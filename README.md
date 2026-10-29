# Sentiment Analysis of IMDb Movie Reviews using ModernBERT with Attention Pooling

B.Tech Project, Department of Computer Science and Engineering, IIIT Bhopal (2026-27)

**Team:** [Student 1 name] ([Scholar no.]) and [Student 2 name] ([Scholar no.])
**Supervisor:** [Supervisor name, designation]

---

## 1. What is this project? (read this first)

- **Task:** the computer reads a movie review and decides if it is **positive** or **negative**.
- **Data:** 50,000 movie reviews from IMDb.
- **Starting point:** a research paper (RoBERTa-BiLSTM, Rahman et al., 2025) that reports **92.36%** accuracy on this data.
- **What we built:** two improved models and tested them on the same data.
  1. **ModernBERT + Attention Pooling** (our main model)
  2. **DeBERTa-v3 + BiLSTM + Attention Pooling** (second model, for comparison)
- **Where it runs:** on **Google Colab** (free GPU). You do not need to install anything on your computer.

## 2. Links

| What | Where |
|---|---|
| Project report | [`docs/Project_Report.docx`](docs/Project_Report.docx) |
| Presentation slides | [`docs/Presentation.pptx`](docs/Presentation.pptx) |
| Code (Colab notebooks) | [`notebooks/`](notebooks/) |
| Dataset | [`data/IMDB Dataset.csv`](data/) |
| Saved outputs of our runs (code + results as PDF) | [`results/`](results/) |
| This repository | `[paste the GitHub link of this repository here]` |

## 3. What is inside each folder

| Folder / file | What it contains |
|---|---|
| `notebooks/01_ModernBERT_AttentionPooling.ipynb` | **Main model.** Trains the model, tests it, and lets you try your own reviews. |
| `notebooks/02_DeBERTaV3_BiLSTM_AttentionPooling.ipynb` | **Second model.** Same steps as notebook 01. |
| `notebooks/03_Benchmark_Comparison.ipynb` | Compares our models with the paper's RoBERTa-BiLSTM on the **same GPU**: accuracy, training time, memory and speed. |
| `data/IMDB Dataset.csv` | The dataset (50,000 reviews, 2 columns: `review`, `sentiment`). |
| `results/` | PDF printouts of our Colab runs (code and outputs) and result files. |
| `docs/` | The project report and the presentation slides. |
| `requirements.txt` | List of Python libraries used. |

## 4. How to run it

### Option A: just look at our results (2 minutes, no running needed)
Open the PDF files in the [`results/`](results/) folder. They show the full code and the output of our runs
(training progress, accuracy, confusion matrix, example predictions).

### Option B: run it yourself on Google Colab
You need a Google account. Follow the steps in order.

1. Go to <https://colab.research.google.com> and sign in.
2. Click **File > Open notebook > GitHub**, paste this repository's link, and open
   `notebooks/01_ModernBERT_AttentionPooling.ipynb`.
   (You can also download the `.ipynb` file from this repository and use **File > Upload notebook**.)
3. Turn on the GPU: **Runtime > Change runtime type > T4 GPU > Save**.
4. Get the dataset into Colab. Use **one** of these two ways:
   - **Easy way:** download `data/IMDB Dataset.csv` from this repository to your computer. When the
     notebook reaches **Section 5**, it shows an upload button. Click it and choose the file.
   - **Automatic way:** before running the notebook, open a new code cell at the top, paste the
     line below with this repository's link, and run it. The notebook then finds the data by itself.
     ```
     !git clone <paste this repository's link here>
     ```
5. Click **Runtime > Run all**.
6. Wait for it to finish. Training takes **several hours** on the free T4 GPU. Keep the browser tab open
   and do not let the computer sleep.
7. At the end the notebook prints the final results: **accuracy, precision, recall, F1-score**, a
   **confusion matrix**, and the sentiment predicted for a few example reviews.

To run the second model, repeat the steps with notebook `02_...`. For the same-GPU comparison with the
paper's model, use notebook `03_...` and read its **Section 4 (settings)** first.

## 5. What you should see

| Model | Test accuracy | F1-score | Training time |
|---|---|---|---|
| Paper: RoBERTa-BiLSTM (as reported) | 92.36% | 92.35% | 170.98 min (5 epochs, RTX 3090 GPU) |
| ModernBERT + Attention Pooling | [fill in]% | [fill in]% | [fill in] min (3 epochs, T4 GPU) |
| DeBERTa-v3 + BiLSTM + Attention Pooling | [fill in]% | [fill in]% | [fill in] min (3 epochs, T4 GPU) |

Your numbers can differ slightly from ours, because GPU calculations are not exactly repeatable.
All results come from one training run on one data split, so the comparison with the paper is only
indicative (its data split, text cleaning and GPU were different from ours).

## 6. Settings we used (for repeating our experiment)

- **Data:** 418 duplicate reviews removed (50,000 becomes 49,582). Split 80% train / 10% validation / 10% test
  (39,665 / 4,958 / 4,959 reviews), with the random seed **42** so the test reviews are always the same.
- **Text cleaning:** only HTML line-break tags and extra spaces are removed.
- **Review length:** at most 512 tokens. For longer reviews we keep the first 128 and the last tokens.
- **Training:** 3 epochs, batch size 16 (two steps combined, so effective 32), AdamW optimizer,
  mixed precision (fast, uses less memory).
- **Learning rates (encoder / new layers):** ModernBERT 3e-5 / 2e-4, DeBERTa 2e-5 / 3e-4.
- **Best model choice:** the epoch with the best validation accuracy. The test set is used only once, at the end.
- **Hardware:** Google Colab, Tesla T4 GPU.

## 7. If something goes wrong

| Problem | What to do |
|---|---|
| "GPU is not available" | Do step 3 again (Runtime > Change runtime type > T4 GPU). |
| "Out of memory" error | In **Section 4** of the notebook, change `MAX_LENGTH = 512` to `384`, or `BATCH_SIZE` to `8`. |
| "Please upload the IMDb CSV file" | Do step 4: upload `IMDB Dataset.csv` when the notebook asks. |
| Red "dependency conflict" lines after the install cell | Usually harmless. If the notebook then fails, choose **Runtime > Disconnect and delete runtime** and run again from the top. |
| Colab disconnected during training | The trained model may be lost. Run again, and copy the saved `.pth` file to Google Drive after training. |

## 8. Running on your own computer (advanced, optional)

The notebooks are written for Colab. To run them locally you need a computer with an NVIDIA GPU
(at least 16 GB memory), then:
1. `pip install -r requirements.txt`
2. In the notebook, change the data cell so `FILE_PATH` points to `data/IMDB Dataset.csv`, and remove the cells that use `google.colab`.

## 9. References

1. M. M. Rahman, A. I. Shiplu, Y. Watanobe, and M. A. Alam, "RoBERTa-BiLSTM: A context-aware hybrid model for sentiment analysis," *IEEE Trans. Emerg. Topics Comput. Intell.*, vol. 9, no. 6, pp. 3788-3805, 2025, doi: 10.1109/TETCI.2025.3572150.
2. A. L. Maas et al., "Learning word vectors for sentiment analysis," in *Proc. ACL-HLT*, 2011, pp. 142-150. (IMDb dataset)
3. B. Warner et al., "Smarter, better, faster, longer: A modern bidirectional encoder for fast, memory efficient, and long context finetuning and inference," arXiv:2412.13663, 2024. (ModernBERT)
4. P. He, J. Gao, and W. Chen, "DeBERTaV3: Improving DeBERTa using ELECTRA-style pre-training with gradient-disentangled embedding sharing," arXiv:2111.09543, 2021.
