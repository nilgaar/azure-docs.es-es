---
title: 'Inicio rápido: Carga masiva de datos con un grupo de SQL dedicado'
description: Use Synapse Studio para realizar una carga masiva de datos en un grupo de SQL dedicado en Azure Synapse Analytics.
services: synapse-analytics
author: kevinvngo
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: quickstart
ms.date: 11/16/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 312c57c103bf733bc72c5de1d22ab3239d5b5e96
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96484717"
---
# <a name="quickstart-bulk-loading-with-synapse-sql"></a>Inicio rápido: Carga masiva con Synapse SQL

La carga de datos es sencilla con el Asistente para la carga masiva en Synapse Studio. Este asistente le guiará a través de la creación de un script de T-SQL con la [instrucción COPY](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true) para cargar datos de forma masiva. 

## <a name="entry-points-to-the-bulk-load-wizard"></a>Puntos de entrada al Asistente para carga masiva

Puede cargar datos de forma masiva fácilmente mediante grupos de SQL dedicados. Solo debe hacer clic con el botón derecho en las siguientes áreas en Synapse Studio:

- Un archivo o una carpeta de una cuenta de Azure Storage asociada al área de trabajo ![Hacer clic con el botón derecho en un archivo o una carpeta de una cuenta de almacenamiento](./sql/media/bulk-load/bulk-load-entry-point-0.png)

## <a name="prerequisites"></a>Requisitos previos

- Este asistente genera una instrucción COPY, que usa el tránsito de Azure AD para la autenticación. Su [usuario de Azure AD debe tener acceso](
./sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples.md#d-azure-active-directory-authentication) al área de trabajo con, al menos, el rol de Azure Colaborador de datos de Storage Blob en la cuenta de ADLS Gen2. 

- Debe tener los [permisos necesarios para usar la instrucción COPY](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true#permissions) y tener permisos de creación de tablas si va a crear una tabla en la que cargar datos.

- El servicio vinculado asociado a la cuenta de Azure Data Lake Storage Gen2 **debe tener acceso al archivo**/**carpeta** que se va a cargar. Por ejemplo, si el mecanismo de autenticación del servicio vinculado es Identidad administrada, la identidad administrada del área de trabajo debe tener, al menos, permiso de lectura de Blob Storage en la cuenta de almacenamiento.

- Si hay una red virtual habilitada en el área de trabajo, asegúrese de que el runtime integrado asociado a la cuenta de Azure Data Lake Storage Gen2 los servicios vinculados para los datos de origen y la ubicación del archivo de error tiene habilitada la creación interactiva. La creación interactiva es necesaria para la detección automática de esquemas, la vista previa del contenido del archivo de origen y la exploración de cuentas de almacenamiento de Azure Data Lake Storage Gen2 en el asistente.

### <a name="steps"></a>Pasos

1. Seleccione en el panel Source storage location (Ubicación de almacenamiento de origen) la cuenta de almacenamiento y el archivo o la carpeta desde donde se va a realizar la carga. El asistente intentará detectar automáticamente los archivos Parquet. Si no se puede confirmar el tipo de archivo Parquet, se usará de forma predeterminada el formato de texto delimitado (CSV).

   ![Selección de la ubicación de origen](./sql/media/bulk-load/bulk-load-source-location.png)

2. Seleccione la configuración del formato de archivo, incluida la cuenta de almacenamiento en la que desea escribir las filas rechazadas (archivo de error). Actualmente solo se admiten archivos .csv y Parquet.

    ![Seleccionar configuración de formato de archivo](./sql/media/bulk-load/bulk-load-file-format-settings.png)

3. Puede seleccionar "Vista previa de los datos" para ver cómo va a analizar el archivo la instrucción COPY para ayudarle en la configuración del formato de archivo. Seleccione "Vista previa de los datos" cada vez que cambie la configuración del formato de archivo para ver cómo va a analizar el archivo la instrucción COPY con la configuración actualizada: ![Vista previa de los datos](./sql/media/bulk-load/bulk-load-file-format-settings-preview-data.png) 

> [!NOTE]  
>
> - El Asistente para carga masiva no admite la vista previa de datos con terminadores de campo de varios caracteres. Si se especifica un terminador de campo de varios caracteres, el Asistente para carga masiva obtendrá una vista previa de los datos de una sola columna. 
> - La especificación de terminadores de fila de varios caracteres se especifica en la instrucción COPY; sin embargo, esto no se admite en el Asistente para carga masiva en el que se producirá un error.

4. Seleccione el grupo de SQL dedicado que va a usar para cargar, incluido si la carga será para una tabla existente o una nueva: ![Seleccionar ubicación de destino](./sql/media/bulk-load/bulk-load-target-location.png)

5. Seleccione "Configure column mapping" (Configurar asignación de columnas) para asegurarse de que tiene la asignación de columnas adecuada. Tenga en cuenta que los nombres de las columnas se detectarán automáticamente si se ha habilitado "Inferir nombres de columna". En el caso de las nuevas tablas, la configuración de la asignación de columnas es fundamental para actualizar los tipos de datos de la columna de destino: ![Configurar la asignación de columnas](./sql/media/bulk-load/bulk-load-target-location-column-mapping.png)

6. Seleccione "Abrir script" y se generará un script de T-SQL con la instrucción COPY para cargar desde un lago de datos: ![Abrir el script SQL](./sql/media/bulk-load/bulk-load-target-final-script.png)

## <a name="next-steps"></a>Pasos siguientes

- Consulte el artículo [Instrucción COPY](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true#syntax) para más información sobre las funcionalidades de COPY
- Consulte el artículo [Introducción a la carga de datos](./sql-data-warehouse/design-elt-data-loading.md#what-is-elt)
