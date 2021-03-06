---
title: Revisión del estado de un trabajo de Azure Import/Export (versión 1) | Microsoft Docs
description: Obtenga información sobre cómo usar los archivos de registro creados por el trabajo de importación o exportación para ver el estado del trabajo.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/26/2017
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 7ef06bb9c5b5010f3fbbe413f98d77cc8519de00
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791807"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Revisión del estado de un trabajo de Azure Import/Export con archivos de registro de copia
Cuando el servicio Microsoft Azure Import/Export procesa las unidades que están asociadas a un trabajo de importación o exportación, escribe archivos de registro de copia en la cuenta de almacenamiento usada para la importación o exportación de blobs. El archivo de registro contiene el estado detallado de cada archivo que se importó o exportó. El servicio devuelve la dirección URL de cada archivo de registro de copia cuando se consulta el estado de un trabajo completado. Para obtener más información, consulte [Trabajo Get](/rest/api/storageimportexport/Jobs/Get).  

## <a name="example-urls"></a>URL de ejemplo

Las siguientes son direcciones URL de ejemplo de los archivos de registro de copia de un trabajo de importación con dos unidades:  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Vea [Formato del archivo de registro del servicio Import-Export](/previous-versions/azure/storage/common/storage-import-export-file-format-log) para consultar el formato de los registros de copia y la lista completa de códigos de estado.  

## <a name="next-steps"></a>Pasos siguientes

 * [Configuración de la herramienta Azure Import/Export](storage-import-export-tool-setup-v1.md)   
 * [Preparación de unidades de disco duro para un trabajo de importación](/previous-versions/azure/storage/common/storage-import-export-tool-preparing-hard-drives-import-v1)   
 * [Reparación de un trabajo de importación](./storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Reparación de un trabajo de exportación](./storage-import-export-tool-repairing-an-export-job-v1.md)