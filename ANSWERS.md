# Midterm 1: answers

GitHub username: Rus7375-design
Session code: practice
Fingerprint: f5c805b9

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used Claude in chat mode. It helped me understand the task, as well as the steps for forking the repository and running the generator in a Colab environment. Using Python, I calculated metrics—specifically recall, MRR, Wilson confidence intervals, and annual costs—based on my own data files. I also drafted the text to justify the chosen approach, prepared a memorandum, and wrote responses for the third part of the assignment. Afterward, I verified the calculated values ​​by cross-referencing them with the formulas provided in the README file.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 48.3 % | 68.3 % | 83.3 % | 0.605 |
| B | 31.7 % | 55.0 % | 81.7 % | 0.486 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 68.3 % | 55.8 % | 78.7 % |
| A, k=5 | 83.3 % | 72.0 % | 90.7 % |
| B, k=3 | 55.0 % | 42.5 % | 66.9 % |
| B, k=5 | 81.7 % | 70.1 % | 89.4 % |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 18304 |
| A, k=5 | 24648 |
| B, k=3 | 11269 |
| B, k=5 | 12923 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

Budget is $15,600, so both A candidates are out: (A,3) costs $18,304 and (A,5) $24,648. Only B fits. Within B, k=5 costs $12,923 and has recall@5 of 81.7 % (70.1-89.4 %), against 55.0 % (42.5-66.9 %) for (B,3). These intervals do not overlap, so k=5 is clearly better. (A,5) scores 83.3 % (72.0-90.7 %), but its interval overlaps (B,5), so no accuracy advantage is shown, and it is over budget anyway. Recall@5 of (B,5) by language: EN 75.0 % (53.1-88.8), RU 80.0 % (58.4-91.9), KK 90.0 % (69.9-97.2), 20 questions each. English users are served worst, but the intervals overlap, so this gap is not proven.

### 1e. Next measurement (at most 60 words)
Test (B,5) on at least 100 questions per language, English first, and also check end-to-end answer correctness. With 20 questions the English interval spans 53-89 %, so we cannot say whether English really lags, or whether paying more for (A,5) would ever be justified.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 92.2 % | KK, degraded | 83.3 % | 66.4 % | 92.7 % | 22010 |
| L | 84.4 % | KK, degraded | 63.3 % | 45.5 % | 78.1 % | 10800 |

Personal-data pages per year sent abroad under H: 509175

### 2b. Memo (at most 300 words)
To: Head of Operations. Recommendation: pipeline L, with human review of Kazakh scans.
Who is served worst. Overall accuracy hides a gap: H 92.2 %, L 84.4 %. For both, the weakest cell is Kazakh on degraded scans: H 83.3 % (66.4-92.7 %), L 63.3 % (45.5-78.1 %). With 30 invoices per cell the intervals overlap, so we cannot prove H is better there, but L's estimate is 20 points lower. Kazakh sellers with poor scans are served worst by both.

Personal data. 31 % of pages carry a name and ИИН, covered by Kazakhstan's law No. 94-V. Under H that is 509,175 pages a year leaving the country. Three questions. Where does the scan go? H: abroad (known). L: our server in Kazakhstan (known). Who stores it? H: the vendor; we do not know whether it keeps images, trains on them or uses sub-processors, and must find out. L: us. For how long? H: unknown, must be fixed in the contract. L: we decide. A vendor's statement is not a compliance review.

Safeguards anyone can verify: (1) a written retention period and a no-training clause, if H is ever used; (2) an access log of who opens stored scans; (3) automatic deletion of scans after processing, tested by inspecting storage; (4) a monthly check of 30 Kazakh degraded invoices by a person, with accuracy reported against the intervals above.

Choice. L costs $10,800 a year against $22,010 for H and keeps personal data in Kazakhstan. Its accuracy is lower, so Kazakh degraded scans go to manual review. Residual risk: about 37 % of those invoices may be extracted wrongly (about 55 % by the interval) and the security of our own server is our responsibility.

## Part 3: Explain your own numbers

### Q1
Number: 0.92
Why (at most 40 words): Kazakh costs 1.95x English in tokens, close to the 2.12x byte ratio, while characters differ only 1.06x. The premium comes mainly from encoding (two bytes per Cyrillic letter), not from a tokenizer penalty.

### Q2
Number: 13.5
Why (at most 40 words): Only English is cut: 576 cl100k tokens are about 592 e5 tokens, over 512. Russian (about 288) and Kazakh (about 171) fit. The cut tail is silently dropped, so facts in it cannot be retrieved.

### Q3
Number: 41.8
Why (at most 40 words): Dividing by T=2 keeps the order, Monday stays first. It flattens the distribution (top falls from 61.1 % at T=1), and every token keeps a non-zero probability, so the sampleable set does not shrink.

### Q4
Number: 20
Why (at most 40 words): The leaderboard prefers X (73 vs 69) because "I don't know" scores zero, so guessing never costs anything. That teaches models to guess confidently, which is hallucination. With the penalty, Y wins, 39 vs 19.

### Q5
Number: 40014
Why (at most 40 words): The first message is paid for 26 times, once per turn, because the model keeps no state. Prompt caching makes the repeated prefix cheaper, but the tokens are still sent and counted.
