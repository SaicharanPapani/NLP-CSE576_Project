How we ran the code:

- Here, we have built the model and saved it in .bin format, after training/fine-tuning.
- After that we have used the saved model and loaded the model file to make predictions on the OOD dataset.


| Task | T5 | BATCH SIZE | EPOCHS | LEARNING RATE | DECAY RATE | TRAIN SAMPLE | TRAINING LOSS | VAL SAMPLE | VAL ACCURACY | TEST SAMPLE | TEST ACCURACY | OOD Sample size | OOD TEST ACC | Time For Training | MODEL SAVED BY	| REMARKS	|	
| ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ |
| Testing the fine-tuned mode with 10 notation - OOD data (4 digits (2000 - 4000)) |	Base |	32 | 25	| 5.00E-04 | -0.3 | 7280 | 0.000012 | 2600 | 99.580793 | 3120 | 99.80867347 | 6000 | 9.70% | around 1.5hr | Saicharan | No Masking, directly fine-tuned T5-Base | 
| Testing the fine-tuned mode with 10 notation - OOD data (5 digits (10000 - 40000)) |	Base |	32 | 25	| 5.00E-04 | -0.3 | 7280 | 0.000012 | 2600 | 99.580793 | 3120 | 99.80867347 | 6000 | 0.016667% | around 1.5hr | Saicharan | No Masking, directly fine-tuned T5-Base | 											
