## CSE576 – OOD Generalization of Numerical Operation (Addition)

Google Drive link to the trained models: https://drive.google.com/drive/folders/1u1glGJbHGzO4jK7ERlBss8K1XsHt40Ad?usp=sharing

**Introduction:**

In majority of the traditional NLP tasks, transformer-based pre-trained language models have performed admirably. On the contrary, they frequently struggle with jobs that demand numerical comprehension which is quite important in most of the applications. This might be due to the fact that, Tokenizers and Pre-training objectives aren't explicitly designed to learn and maintain numeracy.  

There are many instances in which numeric numbers and operations give essential information about the text's content/outcome without being reflected in the text's semantics. So, Sequence-to-Sequence models such as T5 that have outperformed its predecessors for the conventional NLP tasks, are leveraged for this purpose.

**Objective:**

The aim of this project is to use different pre-training and fine-tuning techniques on T5 model to improve its learning with respect to numerical task (specifically addition). Then, evaluate the model’s performance to generalize on Out Of Domain (OOD) data. OOD data refers to the range of numbers on which the model wasn’t trained.

**Experiments & Task Setup:**

Our task at hand is to make a Sequence-to-Sequence model learn addition of two numbers and predict the result. We have used T5, a pre-trained sequence-to-sequence model and fed the input of sequence of tokens and trained it to generate the answers token by token.

For instance, consider “The sum of 232 and 546 is 778”. 

The following part “The sum of 232 and 546 is” would be passed as an input to the final model and the model should be able to predict “778” as the result.

The data required for training has been generated in the above format using random function, where in the two operands have been generated randomly in a specific range.

The models were trained using Google Colab and few systems with high GPU configurations (Nvidia). Please find below a brief workflow that has been followed to achieve the task at hand:

1. Generate data using the random data generator mechanism, which takes length of the dataset as a parameter and also the domain of numbers.
1. Implemented various pre-training and fine-tuning strategies to train the model.
1. Model was tested on inter-domain and OOD data to evaluate its performance.

**Pre-Training + Fine-tuning Methodology:**

**Digit Masking:** Sequence-to-Sequence models are pre-trained using masking strategy to understand the language, which is the same technique that we have incorporated to enable numeracy learning.

In our dataset, we have mixed the strategy of masking single digit and two digits to enable the model to learn the operation better.

Consider the following example,

“The sum of 232 and 546 is 778” is converted to “The sum of 2<mask> and 546 is 778” which will be passed as an input and 32 becomes the label.

The masking strategy has also been implemented in two ways, one in which the masking has been performed on any one of the operands and second one being the masking on either of the operands or result (sum). 

**Additional Fine-tuning Methodology:**

We wanted to analyse and comprehend the ability of a vanilla pre-trained T5 model to understand the numbers. So, we modified the number representations in the following ways:

1. **10e based Representation:**

	The numbers used while generating the dataset would be represented in the following format: 

	Consider **832**. The 10E-based encoding would be **8 10e2 3 10e1 2 10e0**. 

	**Example:** The sum of 23 and 46 is 69 

	**10-based notation:** The sum of 2 10e1 3 10e0 and 4 10e1 6 10e0 is 6 10e1 9 10e0.

2. **10 based Representation:**

	The numbers used while generating the dataset would be represented in the following format: 

	Consider **832**. The 10-based encoding would be **8 100 3 10 2 1**. 

	**Example:** The sum of 23 and 46 is 69 

	**10-based notation:** The sum of 2 10 3 1 and 4 10 6 1 is 6 10 9 1.

**Results & Inferences:**

We will be going through few of the results that have shown good results across our work on understanding T5 model’s ability to learn numeracy. All the results have been updated in the following github repository: <https://github.com/SaicharanPapani/NLP-CSE576_Project>.

**Key Callouts:** 

We have used T5-Base model across the project. We have tried with T5-Small model as well, but did not provide us good results. In our initial experiments, we have found the following hyper-parameters providing us the best results. 

*Batch Size:* 32

*Learning Rate:* 5e-5

*Decay Rate:* -0.3

*Optimizer:* Adafactor (the default optimizer on which the T5 was trained)

1. **Pre-Training + Fine-Tuning Methodology:**

We will first discuss the results corresponding to the pre-training + fine-tuning methodology, based on the normal representation of data (not in any other scientific notation as 10 based etc)

|**Tasks**|**Epochs**|**Train Sample Size**|**Training Loss**|**Validation Sample Size**|**Validation Accuracy**|**Test Sample Size**|**Test Accuracy**|
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|Only Fine-Tune|50|16800|0.00001|12000|83.93%|11200|84.62%|
|Pre-training (masking on either operand) – *Model\_A*|50|16800|0.000006|12000|92.60%|11200|92.76%|
|Fine-tuning the above pre-trained model|50|16800|0.000012|12000|67.83%|11200|67.83%|
|Fine-tuning with additional 20% OOD data on above pretrained model|25|20800|0.004816|12000|78.83%|11200|65.55%|
|Pre-training (masking on either operand or sum) – *Model\_B*|50|16800|0.00001|12000|75.53%|11200|75.74%|
|Fine-tuning using above pre-trained model|50|16800|0.00001|12000|64.38%|11200|64.89%|
	
The results in the table have been color-coded to reflect the difference between the different trained models.

1. Initially, we have directly fine-tuned the vanilla T5 model using the normal representation of data, which will act as a baseline for comparison. We achieved a test accuracy of ~85% on inter-domain data and ~0% on OOD data using this approach. (first row color-coded in light-grey).

2. Next, we went ahead and pre-trained the model with masking on either of the operands. We achieved an accuracy of ~92% in pre-training the model. Later, upon fine-tuning the same model, we achieved a test accuracy of ~68% on inter-domain data and ~0% on OOD data. So, we introduced additional 20% of OOD data to the existing training data which gave us a test accuracy of ~66%. To give a bit more detail into the data, the data used for training consisted of operand with less than or equal to three digits [1 - 1000). (rows color-coded in dark-grey)

We can observe that the test accuracy has slightly gone down by ~2% upon introduction of additional 20% OOD data. Here, the OOD data consists of 4-digit operands. However, to our surprise this model worked pretty well during our evaluation on two different sets of OOD data. 

- OOD Test data with 4-digit operands: We achieved an accuracy of 28.26%. This might be due to the introduction of same range data in the training process (by 20%).
- OOD Test data with 5-digit operands: We achieved an accuracy of 28.27%. 

3. Next, we went ahead and pre-trained the model with masking on either of the operands and result. We achieved an accuracy of ~75% in pre-training the model. Later, upon fine-tuning the same model, we achieved a test accuracy of ~65% on inter-domain data and ~0% on OOD data. Furthermore, we tried introducing the OOD data in the same way as above and the obtained results were not noteworthy to infer from.
	
![image](https://user-images.githubusercontent.com/90519153/144738274-8f044dd5-3bb7-412a-929d-2cf1c4ea3926.png)

**Key Takeaways:**

1. As model performance declined in the fine-tuning, it might be due to model was pre-trained to predict the masked digits to learn the addition operation. But when directly fine-tuned to predict the result model might got confused as it couldn’t find any masked value to predict. So, by introducing mask to complete result or by randomly replacing the results with actual result might work as per BERT pre-training strategy.

1. Based on our insights of our fine-tuned model of two different pre-trained models, Of all wrong predictions in inter-domain dataset, 35% of them are differing with 2, for instance whenever to predict 5, it predicts 7 and vice versa, same with (2,0,4); (6,8); (8,0). Furthermore, when there is a carry to be forwarded in the unit’s digit, it’s failing to calculate properly. 

1. Moreover, Due to T5 was on trained on huge text corpus, it sometimes predicts some random text. For instance: 

“The sum of 37 and 343 is Britanien8turÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒturÄƒ”


2. **Additional Fine-Tuning Methodology:**

Now, we will discuss the results corresponding to the additional fine-tuning strategies that have been implemented to understand the numerical learning ability of models. The major change in this approach is that the model will not be pre-trained by us and moreover, scientific representation of numbers will be used in the dataset.


|**Task**|**Epochs**|**Train Sample Size**|**Training Loss**|**Validation Sample Size**|**Validation Accuracy**|**Test Sample size**|**Test Accuracy**|
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|Fine-Tuning with 10 based notation <br>**(Model-1)**|25|7280|0.000012|2600|99.58|3120|99.81|
|<p>Fine-Tuning model with additional 10% OOD data (10 based notation)</p><p>**(Model-2)**</p>|25|7280|0.000015|2600|99.27|3120|99.28|
|Fine-tuning with 10e based notation<br>**(Model-3)**|10|8344|0.002642|2980|97.63|3576|99.84|

The results in the above table have been color-coded to represent both 10-based notation (**Model-1** and **Model-2**) as well as 10e based notation (**Model-3** and **Model-4**).

1. With respect to the 10-based notation dataset, the model has been directly fine-tuned and we achieved a test accuracy of ~99.8% (Model-1). One important thing to note here is that the dataset used for fine-tuning consisted of operands ranging between 1 and 2000 **(4 digit operands)**. 

	However, when the model was evaluated on OOD data whose operands were in the range of **2000 – 4000**, we achieved an accuracy of ~9.7%.  Though the model was introduced to some 4 digit numbers while fine-tuning, it was unable to generalize upon changing the range of data (which also consisted of 4 digit numbers). The same behaviour was observed when evaluated on 5 digit data as well (~0.02% accuracy)

![image](https://user-images.githubusercontent.com/90519153/144738321-14427afb-6bfc-4096-984f-d7a47ae8ae0e.png)


Model-2 also showed similar behaviour in terms of learning.
![image](https://user-images.githubusercontent.com/90635470/144738605-163e80b8-33ea-4cc9-9475-ae77ee9406aa.png)


2. So, we introduced 10% OOD data while fine-tuning (**Model-2**), which consisted of 5 digit operands. This significantly boosted the performance of the model on 5 digit numbers by giving an accuracy of ~95%. This fine-tuned model was again evaluated on 6 digit numbers, which showed some learning and gave an accuracy of ~2.5%.

3. We also tried 10e based notation with the same dataset that was used in 10-based notation. Though the test accuracy was good, model performed poorly on OOD data (both 4-digits and 5-digits).

![image](https://user-images.githubusercontent.com/90635470/144739021-0e73e576-f1b9-48c4-a216-9c8518a6b0e4.png)

**Key Takeaways:**

1. The 10 based model was not able to learn when the range of the data was changed, though the number of digits remained same.
2. Overall, the learning ability of T5 model was better with respect to 10 based notation as compared to the 10e based notation, upon OOD data.

**Challenges:** 

The main challenge that we faced during the course of the project was with respect to Computation power. The GPU provided by Google Colab doesn’t suffice our requirements, leading to multiple failures during the long run. So, it was very difficult for us to try to train the model for more number of epochs. In order to train the model for more number of epochs, we used T5-Small model but it impacted the accuracy. So, we went ahead with T5-Base model. 

**Conclusion / Future work:** 

We tried training the model using various approaches and number representations like 10e-based & 10-based representation, which showed better accuracies on inter-domain data but failed to generalize on Out-of-Domain data. We observed positive results only when the models were introduced few percent of the OOD data while training or fine-tuning. We observed 10-based representation showed better accuracy than 10e-based representation. We used T5\_base on 13k-15k dataset in this project. In future, T5 large, bigger datasets and a greater number of epochs can be experimented. In this project we used 4 digits in training, in tested upto 5-6 digits. We can try testing with numbers containing more digits in OOD testing dataset. We can also use multi-lingual representation of data to understand the model learning capability.


**Literature Review:**

We have read the below papers to understand Text-to-Text Transfer Model’s Numeracy Learning Ability, Pre-training for Sequence-to-Sequence tasks like Natural Language Generation, Comprehension and Translation, Using Unified Text-to-Text Transformer we studied Limitations of Transfer language  Learning, using simple arithmetic tasks we studied the limits of transformers in detail.

1. https://arxiv.org/abs/2109.04672 
2. https://arxiv.org/abs/1910.13461
3. https://arxiv.org/abs/1910.10683 
4. https://arxiv.org/pdf/2102.13019.pdf
5. Github Repo link: <https://github.com/SaicharanPapani/NLP-CSE576_Project>. 

(Individual work is not reflected in github because we collaborated and integrated separately)

