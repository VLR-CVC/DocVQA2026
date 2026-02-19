<p align="center">
  <img src="./assets/banner.png" alt="DocVQA 2026 Competition Banner" width="100%">
</p>

<h1 align="center">DocVQA 2026 | ICDAR2026 Competition on Multimodal Reasoning over Documents in Multiple Domains</h1>

 <p align="center">
  <a href="https://www.docvqa.org/challenges/2026">
    <img src="https://img.shields.io/badge/🌐_Website-DocVQA.org-orange.svg" alt="Competition Website">
  </a>
  <a href="https://huggingface.co/datasets/VLR-CVC/DocVQA-2026">
    <img src="https://img.shields.io/badge/🤗_Hugging_Face-Dataset-blue.svg" alt="Hugging Face Dataset">
  </a>
  <a href="https://github.com/VLR-CVC/DocVQA2026">
    <img src="https://img.shields.io/badge/GitHub-Eval_Code-black.svg?logo=github&logoColor=white" alt="GitHub Repository">
  </a>
</p>

Building upon previous DocVQA benchmarks, this evaluation dataset introduces challenging reasoning questions over a diverse collection of documents spanning eight domains, including business reports, scientific papers, slides, posters, maps, comics, infographics, and engineering drawings.

By expanding coverage to new document domains and introducing richer question types, this benchmark seeks to push the boundaries of multimodal reasoning and promote the development of more general, robust document understanding models.

## Load & Inspect the Data

```python
from datasets import load_dataset

# 1. Load the dataset
dataset = load_dataset("VLR-CVC/DocVQA-2026", split="val")

# 2. Access a single sample (one document)
sample = dataset[5]

doc_id = sample["doc_id"]
category = sample["doc_category"]
print(f"Document ID: {doc_id} ({category})")

# 3. Access Images
# 'document' is a list of PIL Images (one for each page)
images = sample["document"]
print(f"Number of pages: {len(images)}")
images[0].show()  

# 4. Access Questions and Answers
questions = sample["questions"]
answers = sample["answers"]

# 5. Visualize Q&A pairs for a document
for q, a in zip(questions, answers):
    print("-" * 50)
    print(f"Question: {q['question']}")
    print(f"Answer: {a['answer']}")
    print("-" * 50)

```

## Structure of a Sample

<details>
<summary><b>Click to expand the JSON structure</b></summary>
  
```json
{
    'doc_id': 'comics_1',
    'doc_category': 'comics',
    'document': [,
        <PIL.PngImagePlugin.PngImageFile image mode=RGB size=1240x1754 at 0x7F...>,
        ...
        <PIL.PngImagePlugin.PngImageFile image mode=RGB size=1240x1754 at 0x7F...> 
      
    ],
    'questions': [
        {
            'question_id': 'comics_1_q1', 
            'question': "How many times do people get in the head in Nyoka and the Witch Doctor's Madness?"
        }
    ],
    'answers': [
        {
            'question_id': 'comics_1_q1', 
            'answer': '4'
        }
    ]
}
```
</details>


## Results

<p align="center">
  <img src="./assets/results_chart.jpg" alt="DocVQA 2026 Results Chart" width="80%">
  <br>
  <em>Figure 1: Performance comparison across domains.</em>
</p>


<div align="center">
  <table>
    <thead>
      <tr>
        <th align="left">Category</th>
        <th align="center">Gemini 3 Pro Preview</th>
        <th align="center">GPT-5.2</th>
        <th align="center">Gemini 3 Flash Preview</th>
        <th align="center">GPT-5 Mini</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td align="left"><b>Overall Accuracy</b></td>
        <td align="center"><b>0.375</b></td>
        <td align="center">0.350</td>
        <td align="center">0.3375</td>
        <td align="center">0.225</td>
      </tr>
      <tr>
        <td align="left">Business Report</td>
        <td align="center">0.400</td>
        <td align="center"><b>0.600</b></td>
        <td align="center">0.200</td>
        <td align="center">0.300</td>
      </tr>
      <tr>
        <td align="left">Comics</td>
        <td align="center">0.300</td>
        <td align="center">0.200</td>
        <td align="center"><b>0.400</b></td>
        <td align="center">0.100</td>
      </tr>
      <tr>
        <td align="left">Engineering Drawing</td>
        <td align="center">0.300</td>
        <td align="center">0.300</td>
        <td align="center"><b>0.500</b></td>
        <td align="center">0.200</td>
      </tr>
      <tr>
        <td align="left">Infographics</td>
        <td align="center"><b>0.700</b></td>
        <td align="center">0.600</td>
        <td align="center">0.500</td>
        <td align="center">0.500</td>
      </tr>
      <tr>
        <td align="left">Maps</td>
        <td align="center">0.000</td>
        <td align="center"><b>0.200</b></td>
        <td align="center">0.000</td>
        <td align="center">0.100</td>
      </tr>
      <tr>
        <td align="left">Science Paper</td>
        <td align="center">0.300</td>
        <td align="center">0.400</td>
        <td align="center"><b>0.500</b></td>
        <td align="center">0.100</td>
      </tr>
      <tr>
        <td align="left">Science Poster</td>
        <td align="center"><b>0.300</b></td>
        <td align="center">0.000</td>
        <td align="center">0.200</td>
        <td align="center">0.000</td>
      </tr>
      <tr>
        <td align="left">Slide</td>
        <td align="center"><b>0.700</b></td>
        <td align="center">0.500</td>
        <td align="center">0.400</td>
        <td align="center">0.500</td>
      </tr>
    </tbody>
  </table>
</div>

> [!NOTE]
> **Evaluation Parameters:**
> * **GPT Models:** "High thinking" enabled, temperature set to `1.0`.
> * **Gemini Models:** "High thinking" enabled, temperature set to `0.0`.

> [!WARNING]
> **API Constraints:** > Both models were evaluated via their respective APIs. If a sample fails because the input files are too large, the result counts as a failure. For example, the file input limit for OpenAI models is 50MB, and several comics in this dataset surpass that threshold.

## Dataset Structure

The dataset consists of:
1.  **Images:** High-resolution PNG renders of document pages located in the `images/` directory.
2.  **Annotations:** A Parquet file (`val.parquet`) containing the questions, answers, and references to the image paths.
