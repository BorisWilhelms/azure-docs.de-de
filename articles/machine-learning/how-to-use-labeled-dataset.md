---
title: Erstellen und Untersuchen von Datasets mit Bezeichnungen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Datasetbezeichnungen aus Ihren Azure Machine Learning-Bezeichnungsprojekten exportieren und für Aufgaben für maschinelles Lernen verwenden können.
author: nibaccam
ms.author: nibaccam
ms.service: machine-learning
ms.topic: how-to
ms.date: 01/21/2020
ms.openlocfilehash: 5138109de3f80d405ce95b605714b511480563f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76549346"
---
# <a name="create-and-explore-azure-machine-learning-dataset-with-labels"></a>Erstellen und Untersuchen eines Azure Machine Learning-Datasets mit Bezeichnungen

In diesem Artikel erfahren Sie, wie Sie die Datenbezeichnungen aus einem Azure Machine Learning-Datenbezeichnungsprojekt exportieren und in gängige Formate laden können, z. B. einen Pandas-Datenrahmen zum Untersuchen von Daten oder einen Torchvision-Dataset zur Bildtransformation. 

## <a name="what-are-datasets-with-labels"></a>Was sind Datasets mit Bezeichnungen? 

Azure Machine Learning-Datasets mit Bezeichnungen sind [TabularDatasets](how-to-create-register-datasets.md#dataset-types) mit einer Label-Eigenschaft. Wir werden sie als bezeichnete Datasets bezeichnen. Diese bestimmten Arten von TabularDatasets werden nur als Ausgabe von Azure Machine Learning-Datenbezeichnungsprojekten erstellt. Erstellen Sie ein Datenbezeichnungsprojekt mit [den folgenden Schritten](how-to-create-labeling-projects.md). Machine Learning unterstützt Datenbezeichnungsprojekte für die Bildklassifizierung – mit mehreren Bezeichnungen oder mehreren Klassen – und die Objektidentifikation mit Begrenzungsrahmen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen, bevor Sie beginnen.
* Das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), oder greifen Sie auf [Azure Machine Learning-Studio](https://ml.azure.com/) zu.
    * Installieren des Pakets [azure-contrib-dataset](https://docs.microsoft.com/python/api/azureml-contrib-dataset/?view=azure-ml-py)
* Ein Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).
* Zugriff auf ein Azure Machine Learning-Datenbezeichnungsprojekt. Wenn Sie nicht über ein Bezeichnungsprojekt verfügen, erstellen Sie eines mit [den folgenden Schritten](how-to-create-labeling-projects.md).

## <a name="export-data-labels"></a>Exportieren von Datenbezeichnungen 

Wenn Sie ein Datenbezeichnungsprojekt abschließen, können Sie die Bezeichnungsdaten aus einem Bezeichnungsprojekt exportieren. Auf diese Weise können Sie sowohl den Bezug zu den Daten als auch deren Bezeichnungen erfassen und sie im [COCO-Format](http://cocodataset.org/#format-data) oder als Azure Machine Learning-Dataset exportieren. Verwenden Sie die Schaltfläche **Exportieren** auf der Seite **Projektdetails** Ihres Beschriftungsprojekts.

### <a name="coco"></a>COCO 

 Die COCO-Datei wird im Standardblobspeicher des Azure Machine Learning-Arbeitsbereichs in einem Ordner unter *export/coco* erstellt. 

### <a name="azure-machine-learning-dataset"></a>Azure Machine Learning-Dataset

Sie können im Abschnitt **Datasets** von Ihrem Azure Machine Learning-Studio auf das exportierte Azure Machine Learning-Dataset zugreifen. Die Seite mit **Datasetdetails** bietet auch Beispielcode für den Zugriff auf Ihre Beschriftungen aus Python.

![Exportiertes Dataset](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="explore-labeled-datasets"></a>Untersuchen bezeichneter Datasets

Laden Sie Ihre bezeichneten Datasets in einen Pandas-Datenrahmen oder in ein Torchvision-Dataset, um beliebte Open-Source-Bibliotheken für die Untersuchung von Daten zu nutzen, ebenso wie die von PyTorch bereitgestellten Bibliotheken für die Bildtransformation und das Training.

### <a name="pandas-dataframe"></a>Pandas-Datenrahmen

Mit der Methode [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) der Klasse `azureml-contrib-dataset` können Sie bezeichnete Datasets in einen Pandas-Datenrahmen laden. Installieren Sie die Klasse mit dem folgenden Shellbefehl: 

```shell
pip install azureml-contrib-dataset
```

>[!NOTE]
>Der Namespace „azureml.contrib“ ändert sich häufig, während wir an der Verbesserung des Diensts arbeiten. Daher sollte alles in diesem Namespace als Vorschau und als von Microsoft nicht vollständig unterstützt betrachtet werden.

Wir bieten die folgenden Optionen zur Handhabung von Dateien für Dateidatenströme bei der Konvertierung in einen Pandas-Datenrahmen.
* Herunterladen: Laden Sie Ihre Datendateien in einen lokalen Pfad herunter.
* Einbinden: Binden Sie Ihre Datendateien an einem Bereitstellungspunkt ein. Das Einbinden funktioniert nur für Linux-basiertes Computing, einschließlich Azure Machine Learning-Notebook (virtueller Computer) und Azure Machine Learning Compute.

```Python
import azureml.contrib.dataset
from azureml.contrib.dataset import FileHandlingOption
animal_pd = animal_labels.to_pandas_dataframe(file_handling_option=FileHandlingOption.DOWNLOAD, target_path='./download/', overwrite_download=True)

import matplotlib.pyplot as plt
import matplotlib.image as mpimg

#read images from downloaded path
img = mpimg.imread(animal_pd.loc[0,'image_url'])
imgplot = plt.imshow(img)
```

### <a name="torchvision-datasets"></a>Torchvision-Datasets

Sie können bezeichnete Datasets mit der Methode [to_torchvision()](https://docs.microsoft.com/python/api/azureml-contrib-dataset/azureml.contrib.dataset.tabulardataset?view=azure-ml-py#to-torchvision--) auch von der Klasse `azureml-contrib-dataset` in ein Torchvision-Dataset laden. Um diese Methode zu verwenden, muss [PyTorch](https://pytorch.org/) installiert sein. 

```python
from torchvision.transforms import functional as F

# load animal_labels dataset into torchvision dataset
pytorch_dataset = animal_labels.to_torchvision()
img = pytorch_dataset[0][0]
print(type(img))

# use methods from torchvision to transform the img into grayscale
pil_image = F.to_pil_image(img)
gray_image = F.to_grayscale(pil_image, num_output_channels=3)

imgplot = plt.imshow(gray_image)
```

## <a name="next-steps"></a>Nächste Schritte

* Ein vollständiges Trainingsbeispiel finden Sie im [Notebook für Datasets mit Bezeichnungen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/labeled-datasets/labeled-datasets.ipynb).
