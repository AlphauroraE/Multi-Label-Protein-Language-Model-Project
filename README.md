# Multi-Label-Protein-Language-Model-Project
Course project for CS 6824 Combinatorial and Machine Learning Algorithms for Genomics

Notes:
Use VMR dataset. 
D-R is information about virus, taxonomy
S is id for species
U is common name
X important, what gives sequence. Copy and search in GenBank. Data repo of all sequences
All VMR
/translation is protein seq
Genome is many genes, and each is a sequence
Sequence for virus is concatenate 
Don't need to do that manually. GenBank has API, provide ID, and it can give all the output in a file (FASTA format). BioPython and other libraries to parse that data and turn into pandas quite straightforward.
AA Host Source very broad but we are predicting this
Go to GenBank, source, /host = Sulfolobales and that is what we want to predict
But sometimes the name is not standardized
Can stick to VMR, although high level for now

Col Def second tab in sheets
Clean and remove anything with (S)

https://huggingface.co/facebook/esm2_t33_650M_UR50D
https://huggingface.co/facebook/esm2_t6_8M_UR50D
Use PyTorch and TensorFlow to fine-tune

Read up on ESM2, it is a PLM
It has been pre-trained on all protein sequences, not just viral
If we give it protein sequences and ask it to predict host... some papers mention but don't have empirical analysis
Viral protein sequence is only 1-2% (very small proportion)
ESM2 has not learned enough to answer questions yet
How well does it do? Probably not well. But this could be step 1, just prove that it is not enough even though it has been predicted. 
Step 2 then feed ESM extra knowledge. Train it further. Fine-tuning for host viral - look at demo notebooks. Can also explicitly give it viral protein sequences and fine-tune to show that if I force feed it viral protein sequences, accuracies should be improved. Then we can claim that by default they do not have enough knowledge so to use any downstream, we must pre-train the data. That is the statement I want to make and two experiments prove it to me. 

Reason recommended ESM2 instead of HAVEN is speedy to set up. ESM2 is more established and has more demo tutorials. It is more production-ready. HAVEN is developed newly. 

https://huggingface.co/docs/transformers/model_doc/esm
Basic ESM and info on what it is
ESM for MaskedLM is for pre-training (second exp we talked about)
ESM for SequenceClassification for fine-tuning

First do fine-tuning (SequenceClassification), show it does not perform well
Then do pre-training with MaskedLM and then SequenceClassification it will perform better
Feed viral protein sequences 
Masked Language model in pre-training, these models learn the composition of the viral protein sequences. 
Even showing the models do not do well on viral protein sequences by itself. 
Also doing second part is better. 
Then complete story

Fine-tuning: take entire dataset, break into batches, make model predict on that batch in subset of dataset, see differences, then update model params. Then try another batch, then update model params. Repeat. One complete pass on all batches is called an epoch. Set aside 20% for testing, don't use that in a batch at all. Only 80% becomes batches. Need to do multiple epochs. 8M is still a huge model, need 30-40 passes on one epoch. This code exists already. But what she's saying is that the number of batches/epochs are a parameter which affects length of running experiments. If you do 30 epochs, that's better than 10. But know that I may need to model batch/epoch size. Set epoch to 1, estimate time to do one epoch. Based on that, calculate how much time needed. 

Keep in mind ARC queue
