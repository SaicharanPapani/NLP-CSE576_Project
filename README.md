# NLP-CSE576_Project

## ADD SUB
1. **Pretrain-text source generation** : Generate a lot of sentences using the following template.  
``the sum of <number1> and <number2> is <ans>``
eg : the sum of 123 and 45 is 168
	- [Pretrain-text source generation](/pretrain-text-source-generation/readme.md)
2. **Pretrain data mask creation **: Write multiple functions to introduce masks in the previous
samples generate (randomly select digit / digits to mask)
eg :
the sum of 1<mask>3 and 45 is 168 [operand1 mask]
the sum of 123 and <mask>5 is 168 [operand2 mask]
the sum of 123 and 45 is 1<mask>8 [answer mask]
....
3. **Predict the mask** : Train t5-base model with data generated above to predict the mask value. this will teach the model about the operation rules
4. **Fine-tune** the previously trained model to predict the answer 
eg : input : “the sum of 123
and 45 is “ output : 168
5. Test on same number range data (number of digit of each operand (test) <= number of digit of each operand (train))
6. Test on OOD data (number of digit of each operand (test) > number of digit of each operand (train))