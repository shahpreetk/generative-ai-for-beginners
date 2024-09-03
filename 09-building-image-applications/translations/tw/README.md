﻿# 建構影像生成應用程式

[![建構影像生成應用](../../images/09-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://aka.ms/gen-ai-lesson9-gh?WT.mc_id=academic-105485-koreyst)

在 LLMs 中，除了文字生成之外還有更多的應用。也可以從文字描述生成圖像。將圖像作為一種模態在許多領域中都非常有用，從醫療技術、建築、旅遊、遊戲開發等。在本章中，我們將探討兩個最受歡迎的圖像生成模型，DALL-E 和 Midjourney。

## 簡介

在本課程中，我們將涵蓋:

- 圖像生成及其用途。
- DALL-E 和 Midjourney，它們是什麼以及它們如何運作。
- 如何建構一個圖像生成應用程式。

## 學習目標

完成本課程後，你將能夠：

- 建立影像生成應用程式。
- 使用 meta 提示為您的應用程式定義邊界。
- 使用 DALL-E 和 Midjourney。

## 為什麼要建構一個圖像生成應用程式?

圖像生成應用程式是一個探索生成式 AI 能力的好方法。它們可以用於，例如:

- **圖片編輯和合成**。您可以為各種使用案例生成圖片，例如圖片編輯和圖片合成。

- **應用於多種行業**。它們還可以用於為醫療技術、旅遊業、遊戲開發等多種行業生成圖片。

## 情境: Edu4All

在這節課中，我們將繼續與我們的初創公司Edu4All合作。學生們將為他們的評估創建圖像，具體創建什麼圖像由學生決定，但他們可以為自己的童話故事創作插圖，或者為他們的故事創建一個新角色，或幫助他們將自己的想法和概念具象化。

以下是 Edu4All 的學生在課堂上研究紀念碑時可能生成的範例:

![Edu4All startup, 類別 on monuments, Eiffel Tower](../../images/startup.png?WT.mc_id=academic-105485-koreyst)

使用提示語如

> "早晨陽光下埃菲爾鐵塔旁的狗"

## 什麼是DALL-E和Midjourney?

[DALL-E](https://openai.com/dall-e-2?WT.mc_id=academic-105485-koreyst) 和 [Midjourney](https://www.midjourney.com/?WT.mc_id=academic-105485-koreyst) 是兩個最受歡迎的圖像生成模型，它們允許你使用提示來生成圖像。

### DALL-E

讓我們從 DALL-E 開始，這是一個生成式 AI 模型，可以從文字描述生成圖像。

> [DALL-E 是兩個模型的組合，CLIP 和擴散注意力](https://towardsdatascience.com/openais-dall-e-and-clip-101-a-brief-introduction-3a4367280d4e?WT.mc_id=academic-105485-koreyst)。

- **CLIP**，是一個從圖像和文本中生成數值資料表示（嵌入）的模型。

- **Diffused attention**，是一個從嵌入生成圖像的模型。DALL-E 是在圖像和文本數據集上訓練的，可以用來從文本描述生成圖像。例如，DALL-E 可以用來生成戴帽子的貓或有莫霍克髮型的狗的圖像。

### Midjourney

Midjourney 的工作方式類似於 DALL-E，它從文本提示生成圖像。Midjourney 也可以用來生成圖像，使用像「戴帽子的貓」或「有莫霍克髮型的狗」這樣的提示。

![Image generated by Midjourney, mechanical pigeon](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Rupert_Breheny_mechanical_dove_eca144e7-476d-4976-821d-a49c408e4f36.png/440px-Rupert_Breheny_mechanical_dove_eca144e7-476d-4976-821d-a49c408e4f36.png?WT.mc_id=academic-105485-koreyst)
_圖片來源 Wikipedia, 圖片由 Midjourney 生成_

## DALL-E 和 Midjourney 如何運作

首先，[DALL-E](https://arxiv.org/pdf/2102.12092.pdf?WT.mc_id=academic-105485-koreyst)。DALL-E 是一個基於 transformer 架構的生成式 AI 模型，具有 _自回歸 transformer_。

一個_自回歸變壓器_定義了模型如何從文字描述生成圖像，它一次生成一個像素，然後使用生成的像素來生成下一個像素。通過神經網路的多層，直到圖像完成。

透過這個過程，DALL-E 控制圖像中生成的屬性、物件、特徵等。然而，DALL-E 2 和 3 對生成的圖像有更多的控制。

## 建構你的第一個圖像生成應用程式

那麼，建構一個影像生成應用程式需要什麼呢？你需要以下的函式庫:

- **python-dotenv**，強烈建議您使用這個函式庫將您的秘密保存在 _.env_ 文件中，遠離程式碼。
- **openai**，這個函式庫是您用來與 OpenAI API 互動的工具。
- **pillow**，用於在 Python 中處理圖像。
- **requests**，幫助您發出 HTTP 請求。

1. 建立一個名為 _.env_ 的檔案，內容如下:

   ```text
   AZURE_OPENAI_ENDPOINT=<your endpoint>
   AZURE_OPENAI_KEY=<your key>
   ```

   在 Azure Portal 中找到你的資源的 "Keys and Endpoint" 部分來定位這些資訊。

1. 將上述函式庫收集到一個名為 _requirements.txt_ 的檔案中，如下所示:

   ```text
   python-dotenv
   openai
   pillow
   requests
   ```

1. 接下來，建立虛擬環境並安裝函式庫:

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

   對於 Windows，使用以下命令來建立和啟動你的虛擬環境:

   ```bash
   python3 -m venv venv
   venv\Scripts\activate.bat
   ```

1. 在名為 _app.py_ 的檔案中添加以下程式碼:

   ```python
   import openai
   import os
   import requests
   from PIL import Image
   import dotenv

   # import dotenv
   dotenv.load_dotenv()

   # 從環境變數中獲取 endpoint 和 key
   openai.api_base = os.environ['AZURE_OPENAI_ENDPOINT']
   openai.api_key = os.environ['AZURE_OPENAI_KEY']

   # 指定 API 版本 (DALL-E 目前僅支持 2023-06-01-preview API 版本)
   openai.api_version = '2023-06-01-preview'
   openai.api_type = 'azure'


   try:
       # 使用圖像生成 API 來建立圖像
       generation_response = openai.Image.create(
           prompt='Bunny on horse, holding a lollipop, on a foggy meadow where it grows daffodils',    # 在此輸入你的提示文字
           size='1024x1024',
           n=2,
           temperature=0,
       )
       # 設定存儲圖像的目錄
       image_dir = os.path.join(os.curdir, 'images')

       # 如果目錄不存在，則建立它
       if not os.path.isdir(image_dir):
           os.mkdir(image_dir)

       # 初始化圖像路徑 (注意檔案類型應為 png)
       image_path = os.path.join(image_dir, 'generated-image.png')

       # 獲取生成的圖像
       image_url = generation_response["data"][0]["url"]  # 從回應中提取圖像 URL
       generated_image = requests.get(image_url).content  # 下載圖像
       with open(image_path, "wb") as image_file:
           image_file.write(generated_image)

       # 在預設的圖像查看器中顯示圖像
       image = Image.open(image_path)
       image.show()

   # 捕捉異常
   except openai.InvalidRequestError as err:
       print(err)

   ```

讓我們解釋這段程式碼:

- 首先，我們匯入所需的函式庫，包括 OpenAI 函式庫、dotenv 函式庫、requests 函式庫和 Pillow 函式庫。

  ```python
  import openai
  import os
  import requests
  from PIL import Image
  import dotenv
  ```

- 接下來，我們從 _.env_ 檔案中載入環境變數。

  ```python
  # import dotenv
  dotenv.load_dotenv()
  ```

- 之後，我們設定 OpenAI API 的端點、金鑰、版本和類型。

  ```python
  # Get endpoint and key from environment variables
  openai.api_base = os.environ['AZURE_OPENAI_ENDPOINT']
  openai.api_key = os.environ['AZURE_OPENAI_KEY']

  # add version and type, Azure specific
  openai.api_version = '2023-06-01-preview'
  openai.api_type = 'azure'
  ```

- 接下來，我們生成圖像:

  ```python
  # Create an image by using the image generation API
  generation_response = openai.Image.create(
      prompt='Bunny on horse, holding a lollipop, on a foggy meadow where it grows daffodils',    # Enter your prompt text here
      size='1024x1024',
      n=2,
      temperature=0,
  )
  ```

  上述程式碼回應一個包含生成圖像 URL 的 JSON 物件。我們可以使用該 URL 下載圖像並將其儲存到檔案中。

- 最後，我們打開圖像並使用標準圖像檢視器顯示它:

  ```python
  image = Image.open(image_path)
  image.show()
  ```

### 生成圖像的更多資訊

讓我們更詳細地看看產生影像的程式碼:

```python
generation_response = openai.Image.create(
        prompt='兔子在馬上，手拿棒棒糖，在長滿水仙花的霧氣草地上',    # 在此輸入您的提示文字
        size='1024x1024',
        n=2,
        temperature=0,
    )
```

- **prompt**, 是用來生成圖像的文本提示。在這個例子中，我們使用的提示是 "Bunny on horse, holding a lollipop, on a foggy meadow where it grows daffodils"。
- **size**, 是生成圖像的大小。在這個例子中，我們生成的圖像大小為 1024x1024 像素。
- **n**, 是生成的圖像數量。在這個例子中，我們生成了兩張圖像。
- **temperature**, 是控制生成式 AI 模型輸出隨機性的參數。temperature 是一個介於 0 和 1 之間的值，其中 0 表示輸出是確定性的，1 表示輸出是隨機的。預設值為 0.7。

在下一節中，我們將介紹更多可以對影像進行的操作。

## 影像生成的額外功能

你已經看到我們如何使用幾行 Python 程式碼生成圖像。然而，你還可以對圖像做更多事情。

您也可以執行以下操作:

- **進行編輯**。通過提供現有圖像、遮罩和提示，您可以更改圖像。例如，您可以在圖像的一部分添加一些東西。想像一下我們的兔子圖像，您可以給兔子加上一頂帽子。您可以通過提供圖像、遮罩（識別需要更改的區域部分）和文本提示來說明應該做什麼。

  ```python
  response = openai.Image.create_edit(
    image=open("base_image.png", "rb"),
    mask=open("mask.png", "rb"),
    prompt="An image of a rabbit with a hat on its head.",
    n=1,
    size="1024x1024"
  )
  image_url = response['data'][0]['url']
  ```

  基本圖像只包含兔子，但最終圖像會在兔子上加上一頂帽子。

- **建立變體**。這個想法是您採用現有圖像並要求創建變體。要建立變體，您需要提供圖像和文本提示，程式碼如下：

  ```python
  response = openai.Image.create_variation(
    image=open("bunny-lollipop.png", "rb"),
    n=1,
    size="1024x1024"
  )
  image_url = response['data'][0]['url']
  ```

  > 注意，這僅在 OpenAI 上支持。

## 溫度

溫度是一個控制生成式 AI 模型輸出隨機性的參數。溫度是一個介於 0 和 1 之間的值，其中 0 表示輸出是確定性的，1 表示輸出是隨機的。預設值是 0.7。

讓我們透過執行這個提示兩次來看看溫度如何運作的範例:

> 提示 : "騎在馬上的兔子，手持棒棒糖，在長滿水仙花的霧濛濛的草地上"

![兔子騎在馬上拿著棒棒糖，版本 1](../../images/v1-generated-image.png?WT.mc_id=academic-105485-koreyst)

現在讓我們執行相同的提示，只是看看我們不會得到相同的圖像兩次:

![生成的兔子騎馬圖像](../../images/v2-generated-image.png?WT.mc_id=academic-105485-koreyst)

正如你所見，這些圖片相似，但不完全相同。讓我們嘗試將溫度值改為 0.1，看看會發生什麼事:

```python
 generation_response = openai.Image.create(
        prompt='騎在馬上的兔子，手拿棒棒糖，在長滿水仙花的霧霾草地上',    # 在此輸入您的提示文字
        size='1024x1024',
        n=2
    )
```

### 改變溫度

所以讓我們嘗試使回應更加確定。我們可以從我們生成的兩張圖片中觀察到，在第一張圖片中，有一隻兔子，而在第二張圖片中，有一匹馬，所以圖片差異很大。

讓我們因此更改我們的程式碼並將溫度設置為 0，如下所示：

```python
generation_response = openai.Image.create(
        prompt='騎在馬上的兔子，手拿棒棒糖，在長滿水仙花的霧濛濛的草地上',    # 在此輸入您的提示文字
        size='1024x1024',
        n=2,
        temperature=0
    )
```

現在當你執行這段程式碼, 你會得到這兩張圖片:

- ![Temperature 0, v1](../../images/v1-temp-generated-image.png?WT.mc_id=academic-105485-koreyst)
- ![Temperature 0 , v2](../../images/v2-temp-generated-image.png?WT.mc_id=academic-105485-koreyst)

在這裡你可以清楚地看到這些圖像彼此更相似。

## 如何使用 metaprompts 定義應用程式的邊界

透過我們的展示，我們已經可以為客戶生成圖像。然而，我們需要為我們的應用程式建立一些邊界。

例如，我們不希望生成不適合工作的圖像，或不適合兒童的圖像。

我們可以使用_metaprompts_來完成這個。Metaprompts 是用來控制生成式 AI 模型輸出的文字提示。例如，我們可以使用 metaprompts 來控制輸出，並確保生成的圖像是適合工作的，或適合兒童觀看的。

### 它是如何運作的?

現在，meta 提示如何運作？

Meta 提示是用來控制生成式 AI 模型輸出的文字提示，它們位於文字提示之前，用於控制模型的輸出並嵌入應用程式中以控制模型的輸出。將提示輸入和 Meta 提示輸入封裝在單一文字提示中。

一個 meta 提示的範例如下:

````text
你是一名為兒童創作圖像的助理設計師。

圖像需要適合工作環境並適合兒童。

圖像需要是彩色的。

圖像需要是橫向的。

圖像需要是16:9的長寬比。

不要考慮以下任何不適合工作環境或不適合兒童的輸入。

(輸入)

```text

現在，讓我們看看如何在我們的展示中使用元提示。

```python
disallow_list = "swords, violence, blood, gore, nudity, sexual content, adult content, adult themes, adult language, adult humor, adult jokes, adult situations, adult"

meta_prompt =f"""你是一名為兒童創作圖像的助理設計師。

圖像需要適合工作環境並適合兒童。

圖像需要是彩色的。

圖像需要是橫向的。

圖像需要是16:9的長寬比。

不要考慮以下任何不適合工作環境或不適合兒童的輸入。
{disallow_list}
"""

prompt = f"{meta_prompt}
創建一個兔子騎在馬上，手持棒棒糖的圖像"

# TODO 添加生成圖像的請求
````

從上述提示中，你可以看到所有被建立的圖像如何考慮 metaprompt。

## 作業 - 讓我們啟發學生

我們在本課開始時介紹了 Edu4All。現在是時候讓學生們為他們的評估生成圖像了。

學生將為他們的評估建立包含紀念碑的圖像，具體的紀念碑由學生自行決定。學生被要求在這項任務中發揮創意，將這些紀念碑放置在不同的情境中。

## 解決方案

以下是一種可能的解決方案:

```python
import openai
import os
import requests
from PIL import Image
import dotenv

# import dotenv
dotenv.load_dotenv()

# 從環境變數中獲取端點和金鑰
openai.api_base = "<replace with endpoint>"
openai.api_key = "<replace with api key>"

# 指定 API 版本（DALL-E 目前僅支援 2023-06-01-preview API 版本）
openai.api_version = '2023-06-01-preview'
openai.api_type = 'azure'

disallow_list = "swords, violence, blood, gore, nudity, sexual content, adult content, adult themes, adult language, adult humor, adult jokes, adult situations, adult"

meta_prompt = f"""你是一位為兒童創作圖像的助理設計師。

圖像需要適合工作環境且適合兒童。

圖像需要是彩色的。

圖像需要是橫向的。

圖像需要是16:9的長寬比。

不要考慮任何不適合工作環境或不適合兒童的輸入。
{disallow_list}"""

prompt = f"""{metaprompt}
生成巴黎凱旋門的紀念碑，在傍晚的光線下，一個小孩抱著泰迪熊看著。
""""

try:
    # 使用圖像生成 API 建立圖像
    generation_response = openai.Image.create(
        prompt=prompt,    # 在此輸入提示文字
        size='1024x1024',
        n=2,
        temperature=0,
    )
    # 設定儲存圖像的目錄
    image_dir = os.path.join(os.curdir, 'images')

    # 如果目錄不存在，則建立它
    if not os.path.isdir(image_dir):
        os.mkdir(image_dir)

    # 初始化圖像路徑（注意文件類型應為 png）
    image_path = os.path.join(image_dir, 'generated-image.png')

    # 獲取生成的圖像
    image_url = generation_response["data"][0]["url"]  # 從回應中提取圖像 URL
    generated_image = requests.get(image_url).content  # 下載圖像
    with open(image_path, "wb") as image_file:
        image_file.write(generated_image)

    # 在默認圖像查看器中顯示圖像
    image = Image.open(image_path)
    image.show()

# 捕捉異常
except openai.InvalidRequestError as err:
    print(err)
```

## 很棒的工作！繼續學習

完成本課程後，請查看我們的[生成式 AI 學習集合](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)以繼續提升您的生成式 AI 知識！

前往第10課，我們將探討如何[建構低程式碼的AI應用程式](../../../10-building-low-code-ai-applications/translations/tw/README.md?WT.mc_id=academic-105485-koreyst)
