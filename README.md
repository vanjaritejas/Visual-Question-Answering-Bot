# Visual Question Answering with ViT

**Project author:** Tejas Prashant Vanjari  
**Status:** In development

Visual Question Answering (VQA) combines computer vision and natural language processing to answer questions about images. This project investigates a specific question: **How much does seeing the image improve the answer compared with reading the question alone?**

For example, given the question “What color is the car?”, a question-only model might guess a common color. The multimodal model receives the same question along with the image and can use visual features to predict the answer.

## Table of Contents

1. [Dataset and Preprocessing](#dataset-and-preprocessing)
2. [Model Architecture](#model-architecture)
3. [Evaluation and Results](#evaluation-and-results)
4. [Streamlit Demo](#streamlit-demo)
5. [Limitations](#limitations)
6. [Roadmap](#roadmap)
7. [Acknowledgments](#acknowledgments)

## Dataset and Preprocessing

The project will use [VQA v2](https://visualqa.org/download.html), which pairs images from MS COCO with natural-language questions and answers from human annotators.

1. Start with a small sample and verify that each question is paired with the correct image and annotations.
2. Build a vocabulary of frequent answers using only the training data. Both models will predict from this same vocabulary.
3. Tokenize questions for DistilBERT and preprocess images for ViT.
4. Keep the official training and validation splits separate.

## Model Architecture

| Model | Inputs | Planned architecture |
| --- | --- | --- |
| Question-only baseline | Question | DistilBERT text embedding → answer classifier |
| Image + question model | Image and question | ViT image embedding + DistilBERT text embedding → concatenation → answer classifier |

The first version will use frozen pretrained encoders and train the answer classifiers. Caching the embeddings will make it possible to test classifier changes without repeatedly running both encoders.

## Evaluation and Results

Both models will be evaluated on the same validation examples using the [official VQA metric](https://visualqa.org/evaluation.html), which compares predictions with multiple human answers.

The evaluation will report overall scores and a breakdown by **yes/no**, **number**, and **other**. I’ll also inspect cases where the image helped, where both models relied on common answers, and where the expected answer was outside the vocabulary.

One qualitative test will use the same question with two different images. For example, I’ll ask “What color is the car?” about a red car and a blue car to see whether the multimodal model changes its prediction appropriately.

**Results:** Experiments are pending. Validation scores and example predictions will be added after training and evaluation.

## Streamlit Demo

The planned demo will let users upload an image, enter a question, and see predictions from both models side by side. This will make it easy to try the same question on different images or ask several questions about the same image.

## Limitations

- **Fixed answer vocabulary.** The classifiers can only predict answers included in the training vocabulary.
- **Answer frequency bias.** Both models may favor common answers, especially when the question follows a familiar pattern.
- **Fine visual details.** A single image embedding may miss small objects, spatial relationships, or details needed for counting.
- **Dataset scope.** Results from a small subset will reflect that subset. The final report will state the sample sizes and selection method.

## Roadmap

- [ ] Verify image, question, and answer loading on a small VQA v2 sample.
- [ ] Train the question-only baseline.
- [ ] Add ViT image features and train the multimodal classifier.
- [ ] Compare both models and document their errors.
- [ ] Build the Streamlit demo and add reproducible setup instructions.

## Acknowledgments

I acknowledge **Professor Dipanjan Saha**, who teaches **EECE/CS 5639 Computer Vision** at **Northeastern University**, where I serve as a teaching assistant.

I also acknowledge the creators of VQA v2 and MS COCO for the datasets used in this project.
