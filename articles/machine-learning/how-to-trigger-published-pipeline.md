---
title: Desencadenamiento de la canalización de ML para datos nuevos
titleSuffix: Azure Machine Learning
description: Aprenda a desencadenar la ejecución de una canalización de Azure Machine Learning mediante Azure Logic Apps para responder a los nuevos datos.
services: machine-learning
author: NilsPohlmann
ms.author: nilsp
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.date: 02/07/2020
ms.topic: conceptual
ms.custom: how-to, contperfq4
ms.openlocfilehash: 20d44fd3150f9da31e9c242017e597d4f46e4d5d
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94963936"
---
# <a name="trigger-a-run-of-a-machine-learning-pipeline-from-a-logic-app"></a>Desencadenamiento de una ejecución de una canalización de Machine Learning desde una aplicación lógica

Desencadene la ejecución de la canalización de Azure Machine Learning cuando aparezcan nuevos datos. Por ejemplo, podría desencadenar la canalización para entrenar un nuevo modelo cuando aparezcan nuevos datos en la cuenta de almacenamiento de blobs. Configure el desencadenador con [Azure Logic Apps](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Prerrequisitos

* Un área de trabajo de Azure Machine Learning. Para más información, consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* El punto de conexión de REST de una canalización de Machine Learning publicada. [Cree y publique la canalización](how-to-create-your-first-pipeline.md). A continuación, busque el punto de conexión de REST de la canalización publicada mediante el identificador de canalización:
    
     ```
    # You can find the pipeline ID in Azure Machine Learning studio
    
    published_pipeline = PublishedPipeline.get(ws, id="<pipeline-id-here>")
    published_pipeline.endpoint 
    ```
* [Azure Blob Storage](../storage/blobs/storage-blobs-overview.md) para almacenar los datos.
* [Un almacén de datos](how-to-access-data.md) en el área de trabajo que contenga los detalles de la cuenta de almacenamiento de blobs.

## <a name="create-a-logic-app"></a>Crear una aplicación lógica

Ahora cree una instancia de [aplicación lógica de Azure](../logic-apps/logic-apps-overview.md). Si lo desea, [use un entorno de servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) y [configure una clave administrada por el cliente](../logic-apps/customer-managed-keys-integration-service-environment.md) para su uso por la aplicación lógica.

Cuando se haya aprovisionado la aplicación lógica, use estos pasos para configurar un desencadenador para la canalización:

1. [Cree una identidad administrada asignada por el sistema](../logic-apps/create-managed-service-identity.md) para conceder a la aplicación acceso al área de trabajo de Azure Machine Learning.

1. Vaya a la vista del Diseñador de aplicación lógica y seleccione la plantilla de aplicación lógica vacía. 
    > [!div class="mx-imgBorder"]
    > ![Plantilla vacía](media/how-to-trigger-published-pipeline/blank-template.png)

1. En el Diseñador, busque **blob**. Seleccione el desencadenador **When a blob is added or modified (properties only)** (Cuando se agrega o modifica un blob [solo propiedades]) y agregue este desencadenador a la aplicación lógica.
    > [!div class="mx-imgBorder"]
    > ![Agregar desencadenador](media/how-to-trigger-published-pipeline/add-trigger.png)

1. Rellene la información de conexión de la cuenta de almacenamiento de blobs que quiere supervisar para agregar o modificar blobs. Seleccione el contenedor que se va a supervisar. 
 
    Elija los valores de **Intervalo** y **Frecuencia** para buscar actualizaciones que funcionen en su caso.  

    > [!NOTE]
    > Este desencadenador supervisará el contenedor seleccionado, pero no supervisará las subcarpetas.

1. Agregue una acción HTTP que se ejecutará cuando se detecte un blob nuevo o modificado. Seleccione **+ Nuevo paso** y busque y seleccione la acción HTTP.

  > [!div class="mx-imgBorder"]
  > ![Buscar acción HTTP](media/how-to-trigger-published-pipeline/search-http.png)

  Para configurar la acción, use los siguientes valores:

  | Configuración | Value | 
  |---|---|
  | Acción HTTP | POST |
  | URI |el punto de conexión a la canalización publicada que encontró como [requisito previo](#prerequisites). |
  | Modo de autenticación | Identidad administrada |

1. Configure la programación para establecer el valor de cualquier elemento [DataPath PipelineParameters](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-showcasing-datapath-and-pipelineparameter.ipynb) que pueda tener:

    ```json
    "DataPathAssignments": { 
         "input_datapath": { 
                            "DataStoreName": "<datastore-name>", 
                            "RelativePath": "@triggerBody()?['Name']" 
    } 
    }, 
    "ExperimentName": "MyRestPipeline", 
    "ParameterAssignments": { 
    "input_string": "sample_string3" 
    },
    ```

    Use el elemento `DataStoreName` que agregó al área de trabajo como [requisito previo](#prerequisites).
     
    > [!div class="mx-imgBorder"]
    > ![Configuración de HTTP](media/how-to-trigger-published-pipeline/http-settings.png)

1. Seleccione **Guardar** y la programación ya estará lista.

> [!IMPORTANT]
> Si utiliza el control basado en roles de Azure (Azure RBAC) para administrar el acceso a la canalización, [establezca los permisos del escenario de canalización (entrenamiento o puntuación)](how-to-assign-roles.md#common-scenarios).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

> [!div class="nextstepaction"]
> [Uso de canalizaciones de Azure Machine Learning para la puntuación por lotes](tutorial-pipeline-batch-scoring-classification.md)

* Más información sobre las [canalizaciones](concept-ml-pipelines.md)
* Más información sobre la [exploración de Azure Machine Learning con Jupyter](samples-notebooks.md)

