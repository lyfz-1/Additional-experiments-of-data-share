# Additional experiments of the "data-share"

**Note:** This repository is a supplement to the "data-share" repository, which mainly includes additional experiments, results, and explanations. The link to the "data-share" repository is: [lyfz-1/data-share: Anonymous data share for ASE 2024. (github.com)](https://github.com/lyfz-1/data-share)



## (Reviewer #404A->Q1)

**Q: How do you categorise a commit as explicit or implicit?**

A: The definitions of explicit and implicit commit messages are: Explicit commit messages are surface-level descriptions of code changes and directly reflect code modifications. Implicit commit messages are messages that can be summarized after understanding or reasoning about the logic of the code changes, and they reflect the deep meaning of the code changes.

In a measurable way, the token overlap degree (between commit message and code diff) can be used as one of the measurable features to distinguish between explicit and implicit commit messages. 

The token overlap degree d can be expressed as **d = n / N**, where the n denotes the number of tokens in the commit message that appear in the diff token sequence, and the N denotes the number of tokens in the commit message.

The distribution of token overlap degrees for explicit and implicit commit messages  is as follows:

<img src="https://s2.loli.net/2024/07/26/xvBDiaTJboXE16L.png" alt="explicit.png" style="zoom:80%;" />

<img src="https://s2.loli.net/2024/07/26/U3pVdrakWcnlsAR.png" alt="implicit.png" style="zoom:80%;" />

The distribution of token overlap degrees for explicit and implicit commit messages has a clear dividing line when applying a statistical approach called Otsu's Method (N. Otsu. IEEE transactions on systems, man, and cybernetics 1979). Otsu's Method is a technique for automatically finding the best segmentation threshold between two value distributions by maximizing the inter-class variance. As shown below: 

<img src="https://s2.loli.net/2024/07/26/tXB7zHORi6ku3ow.png" alt="ex_im.png" style="zoom:80%;" />

We found that the optimal segmentation threshold is 0.125. This suggests that a token overlap degree of 0.125 can statistically distinguish between explicit and implicit commit messages to some extent.

For a better understanding, we introduce a case analysis:

![diff_example_new.png](https://s2.loli.net/2024/07/26/TNGsDqiYk53vVOQ.png)

For the code changes in checkBill.java, the corresponding commit message is "**add some new checking rules**". 

This commit message belongs to the "what" category as defined by Tian et al. However, according to our definition, it should belong to the implicit commit message, which highly generalizes the code changes. If the code changes were expressed as an explicit commit message, it would look something like this: “make sure expense is greater than 0 and less than 200, and credit is equal to 100”.



## (Reviewer #404A->Q2)

**Q: Why did the authors use GitHub API rather than clone the repositories? Any benefits?**

A: We are very sorry for the misrepresentation in the paper. In fact, for data collection, we first cloned the Java repositories that meet the requirements, and then used the git library, a Python library, to obtain the commit list. The corresponding script for data collection and cleaning can be found in our open source package: [lyfz-1/data-share: Anonymous data share for ASE 2024. (github.com)](https://github.com/lyfz-1/data-share)



## (Reviewer #404A->Q3) and (Reviewer #404B->Q1)

**(Reviewer #404A->Q3): Why did the authors focus on learning-based models rather than LLMs?**

**(Reviewer #404B->Q1): Why did you choose these models and not more recent LLMs in the evaluation?**

A: We added experiments on two other recent LLMs (GPT4o-mini and Llama3-70B): 

|            LLM            |  Prompt  |  BLEU_M2  | BLEU_M3  | BLEU_DC  |  ROUGE-L  |  METEOR   |
| :-----------------------: | :------: | :-------: | :------: | :------: | :-------: | :-------: |
| ChatGPT （gpt-3.5-turbo） | Explicit | **9.63**  | **4.47** | **2.79** | **20.84** | **14.09** |
|                           | Implicit |   6.20    |   2.69   |   1.60   |   9.77    |   6.81    |
|        GPT4o-mini         | Explicit | **10.16** | **4.73** | **3.05** | **24.10** | **16.38** |
|                           | Implicit |   6.64    |   2.84   |   1.72   |   11.43   |   7.95    |
|        Llama3-70B         | Explicit | **8.44**  | **3.83** | **2.55** | **24.08** | **15.35** |
|                           | Implicit |   6.22    |   2.59   |   1.62   |   12.74   |   8.16    |

The results show that the three recent LLMs we evaluated are better at generating explicit commit messages. This is consistent with the conclusion we obtained in RQ2. What's more, the results also show that LLMs do not perform better than learning-based SOTA models on commit message generation.



## (Reviewer #404C->Q2)

**Q: Why was the size of the datasets chosen to be 50,000, and how were the manually verified 10,000 used in them?**

A: After investigating the size of a number of existing publicly available datasets (as shown in the following table), a medium size was chosen for our dataset size, i.e., 50,000.

| Datasets       | Size   | References                                                   |
| -------------- | ------ | ------------------------------------------------------------ |
| NNGen_data     | 27144  | Liu Z, Xia X, Hassan AE, Lo D, Xing Z, Wang X (2018) Neural-machine-translation-based commit message generation: how far are we? In: ASE. ACM, pp 373–384 |
| CmtGen_data    | 32208  | Jiang S, McMillan C (2017) Towards automatic generation of short summaries of commits. In: Proceedings of the 25th international conference on program comprehension, ICPC 2017, Buenos Aires, Argentina, May 22-23, 2017 |
| PtrGNCMsg_data | 32663  | Liu Q, Liu Z, Zhu H, Fan H, Du B, Qian Y (2019) Generating commit messages from diffs using pointergenerator network. In: MSR. IEEE/ACM, pp 299–309 |
| MultiLang_data | 38734  | Loyola P, Marrese-Taylor E, Matsuo Y (2017) A neural architecture for generating natural language descriptions from source code changes. In: ACL (2). Association for Computational Linguistics, pp 287–292 |
| CODISUM_data   | 90661  | Xu S, Yao Y, Xu F, Gu T, Tong H, Lu J (2019) Commit message generation for source code changes. In: IJCAI, pp 3975–3981. [ijcai.org](http://ijcai.org) |
| CoRec_data     | 107448 | Wang H, Xia X, Lo D, He Q, Wang X, Grundy J (2021) Context-aware retrieval-based deep commit message generation. ACM Trans Softw Eng Methodol 30(4):56:1–56:30 |



## (Reviewer #404C->Q3) and (Reviewer #404A->statistical testing)

**Q: Were statistical tests performed to ensure the robustness of close results, especially in Tables 4 and 7?**

A: We conducted a Mann-Whitney U test in Tables 4 and 7 in the paper. **Note:** The values in the following 4 tables represent the p-value of the Mann-Whitney U test. A p-value less than 0.05 indicates that the difference is statistically significant. P-values less than 0.05 are highlighted in bold.

**For Table 7 ("Mixed" strategy VS "Diversion" strategy):** 

| Metric  | CODISUM                    | PtrGNCMsg                  | CoreGen                    | FIRA                       | CCRep                     | CCT5               |
| ------- | -------------------------- | -------------------------- | -------------------------- | -------------------------- | ------------------------- | ------------------ |
| BLEU_M2 | **3.1869342339837924e-11** | **1.3834425358716458e-24** | **0.0010202316334497743**  | **1.017543604726252e-14**  | **0.00396342496002927**   | 0.4155530939376887 |
| BLEU_M3 | **1.4212563376230048e-11** | **1.560373833951429e-25**  | **0.003097741267055848**   | **1.654227256741303e-16**  | **0.0042407858287169305** | 0.4620915336002417 |
| BLEU_DC | **1.294241174596898e-11**  | **4.295706475373055e-25**  | **8.93810894317211e-06**   | **3.2793987605115055e-15** | **0.037448135283566726**  | 0.7946265743664143 |
| ROUGE-L | **4.8077958350037744e-08** | **4.086299394020682e-27**  | **7.523697617188568e-10**  | **4.509574064986868e-55**  | 0.08285513888985059       | 0.7488101395977732 |
| METEOR  | **3.877481570472233e-14**  | **7.066870936925365e-08**  | **1.4977618582430074e-11** | **8.66706243308982e-48**   | 0.8332363528015287        | 0.4995754608958072 |



**For Table 4:**

**(1) ETET VS ITIT:**

| Metric  | CODISUM                    | PtrGNCMsg                  | CoreGen                    | FIRA                      | CCRep                      | CCT5                       |
| ------- | -------------------------- | -------------------------- | -------------------------- | ------------------------- | -------------------------- | -------------------------- |
| BLEU_M2 | **1.4743592393427577e-44** | **0.0002003067235705041**  | **0.011081309867243578**   | **0.001456222911929299**  | **4.3306773094728524e-47** | **1.6114822473549285e-72** |
| BLEU_M3 | **3.048361960483622e-37**  | **9.248845540184591e-07**  | 0.27964602075876666        | **0.0009120864905468842** | **3.200340578568746e-37**  | **4.097595661177272e-59**  |
| BLEU_DC | **3.0550744239747177e-24** | **4.400754905857327e-09**  | 0.44444892306676653        | **5.898204665914355e-06** | **5.231250982075904e-21**  | **6.49927750628513e-40**   |
| ROUGE-L | **2.473937984273837e-35**  | **0.007240550138170831**   | **0.013674167144995432**   | 0.9336615611185645        | **8.386280788564239e-29**  | **3.2149656961631426e-41** |
| METEOR  | **9.25309080185703e-65**   | **2.1773561013973135e-27** | **1.8390030889665683e-22** | **8.64427214786812e-136** | **9.113497430417396e-10**  | **5.92526264014122e-45**   |

**(2) ETET VS MTET:**

| Metric  | CODISUM                     | PtrGNCMsg                | CoreGen                   | FIRA                       | CCRep                      | CCT5                     |
| ------- | --------------------------- | ------------------------ | ------------------------- | -------------------------- | -------------------------- | ------------------------ |
| BLEU_M2 | **7.295837418866878e-10**   | **0.015246605636226209** | **0.009133015287572354**  | **6.589825973495234e-09**  | **9.103548389851037e-08**  | 0.11919571122769818      |
| BLEU_M3 | **3.124781282334522e-10**   | **0.013176593333038023** | **0.016639656285522515**  | **7.862460169348289e-09**  | **8.596170772276451e-08**  | 0.14625474359648974      |
| BLEU_DC | **5.968273765218052e-10**   | **0.010274362277921088** | **0.0006580678027226779** | **4.717772062009675e-09**  | **8.195398540994544e-07**  | 0.0918282780159528       |
| ROUGE-L | **5.964411308733414e-09**   | 0.057357293689685555     | **0.0001738381174403388** | **4.6745745587510086e-06** | **1.9718406332706472e-07** | **0.029791305879571288** |
| METEOR  | **3.1780079871962773e-307** | **0.049576031557420436** | 0.35487571106382565       | **4.160165961261853e-205** | **4.1802978314833526e-09** | **0.012296263158770422** |

**(3) ITIT VS MTIT:**

| Metric  | CODISUM                     | PtrGNCMsg                  | CoreGen                     | FIRA                       | CCRep                       | CCT5                       |
| ------- | --------------------------- | -------------------------- | --------------------------- | -------------------------- | --------------------------- | -------------------------- |
| BLEU_M2 | **0.015812954378920503**    | **3.592269741334151e-51**  | **2.0672667227224334e-127** | **5.29906189263062e-14**   | **2.264348738504314e-44**   | **1.717807618562481e-70**  |
| BLEU_M3 | **0.006198734051592704**    | **1.1482875443183121e-54** | **2.6823423035182097e-128** | **2.339826937181894e-12**  | **1.9973378104754476e-44**  | **7.644494780578593e-68**  |
| BLEU_DC | **0.000341798494617502**    | **2.639577232971103e-54**  | **1.0409618880777701e-132** | **3.68269587319769e-24**   | **1.513281361227831e-55**   | **7.567054160329334e-79**  |
| ROUGE-L | **4.606878566209168e-05**   | **1.0225926561785575e-62** | **1.4070083077634943e-116** | **4.7509145915160467e-33** | **1.5953923391753824e-59**  | **1.5593983167920862e-91** |
| METEOR  | **2.6817143508429257e-216** | **4.164144264468351e-176** | **8.749099712237379e-289**  | **7.874326216297881e-184** | **2.8058241970123486e-148** | **7.549297448458998e-217** |

The results show that most of the performance changes in Tables 4 and 7 are statistically significant.



## (Reviewer #404A->the inconsistency of classifiers in RQ1 and RQ4)

We supplemented the results of another five models used in RQ4 (we made them bold in the "Models" column) for the RQ1 experiment:

|         Models          |  Accuracy  | Precision  |   Recall   |     F1     |
| :---------------------: | :--------: | :--------: | :--------: | :--------: |
|         **KNN**         |   59.18%   |   58.02%   |   66.57%   |   61.98%   |
|      **AdaBoost**       |   68.37%   |   67.23%   |   71.88%   |   69.44%   |
|         **SVM**         |   74.65%   |   75.40%   |   73.41%   |   74.32%   |
|      Random Forest      |   78.32%   |   77.59%   |   79.71%   |   78.62%   |
|        LightGBM         |   80.91%   |   80.26%   |   82.06%   |   81.12%   |
| **logistic Regression** |   81.30%   |   81.58%   |   80.86%   |   81.20%   |
|        CatBoost         |   84.22%   |   84.11%   |   84.42%   |   84.25%   |
|         XGBoost         |   84.69%   |   84.54%   |   84.95%   |   84.73%   |
|         **MLP**         |   91.77%   |   90.64%   |   92.21%   |   91.46%   |
|          LSTM           |   94.84%   |   93.79%   |   90.78%   |   92.77%   |
|         Bi-LSTM         | **96.12%** | **94.02%** | **92.01%** | **93.24%** |

The results show that the best performer is still the Bi-LSTM-based classifier.