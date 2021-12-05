How we ran the code:

- Here, we have built the model and saved it in .bin format, after training/fine-tuning.
- After that we have used the saved model and loaded the model file to make predictions on the OOD dataset.

| Task | T5 | BATCH SIZE | EPOCHS | LEARNING RATE | DECAY RATE | TRAIN SAMPLE | TRAINING LOSS | VAL SAMPLE | VAL ACCURACY | TEST SAMPLE | TEST ACCURACY | OOD Sample size | OOD TEST ACC | Time For Training | MODEL SAVED BY	| REMARKS	|	
| ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ |
| Testing the fine-tuned model with 10 notation with 10% OOD data - OOD data (5 digits 10000 - 40000)| Base | 32 | 25	| 5.00E-04 | -0.3 | 7280(consist of 10% of OOD data) | 0.000015 | 2600 | 99.275915 | 3120 | 99.26658163 | 6000(i.i.d data) | 95.51% | around 6hr | Saicharan | No Masking, directly fine-tuned T5-Base | 												
| Testing the fine-tuned model with 10 notation with 10% OOD data - OOD data (6 digits 10000 - 40000)| Base | 32 | 25	| 5.00E-04 | -0.3 | 7280(consist of 10% of OOD data) | 0.000015 | 2600 | 99.275915 | 3120 | 99.26658163 | 2000 | 2.4% | around 1hr | Saicharan | No Masking, directly fine-tuned T5-Base | 
