### Optical-Character-Recognition-OCR-using-HMM

## Problem Statement
To run the program :  
```
python3 ./image2text.py train-image-file.png train-text.txt test_images/test-0-0.png
```
OCR stands for Optical Character Recognition and refers to a software technology that electronically identifies text (written or printed) inside an image file or physical document, such as scanned document, and converts it into a machine readable text for to be used for data processing. It is also known as "text recognition".  
In short, optical character recognition software helps convert images or physical documents into a searchable form. Examples of OCR are text extraction tools, PDF to .txt converters and Google's image search function.  
Using the versatility of HMM, we applied them to the problem of Optical Character Recognition. Our goal is to identify text in an image where the font and font size is known ahead of time. Modern OCR is great at recogniziting documents, but rather poor when recognizing individual (isolated) characters. OCR's algorithm can resolve ambiguities in recognition by using statistical constraints of English. These constraints can be incorporated very naturally using HMM.

## Program Description
We are given a set of images from which we have to extract the text and print out the correct text as our output.
Initially, we calculate the emission, transition and initial state probabilities for the data.
The initial state probabilities for the training characters are calculated from a training text file.
```
Initial Probability of a Character = (number of times the character is the first alphabet of a word) / (total number of words)
```
Thus, in our case, to calculate the initial probability of a character, we have used the technique called "Laplace Smoothing" which we used for our Part 1 too which is used to handle the problem of 0 probability in Naive Bayes.

```
P(w'|positive) = (frequency of the first word in the sentence w' and y = positive + α) / (N + α * K)
```
Here, 
α (alpha) reperesents the smoothing parameter (we've assigned 3 in our case).
K represents the number of dimensions(features) in the data (K = 2 in our case)
N represents the frequency of the first word with y=positive

We've calculated the transition probabily as:
```
P(S_i|S_{i-1}) = number of times state i-1 is followed by state i/(number of occurences of state i-1)
```
To calculate the emission probabilities, we have used each observation, where each observation is a group of pixels

```
Emission probability = P(all observed pixels | letter) = P(pixel1 | a)P(pixel2 | a)...P(pixelN | a)
```
and the pixel is either present or absent (white or black) i.e 1 or 0

So, we can write the emission probability as:
```
P(all obsereved pixels | letter) = (1) ^ (no. of black pixels) * (0) ^ (no. of white pixels)
```

We also know that some percentage of the pixels are noisy(say n%). that means after using naive bayes we can assume that the noisy pixels of the observed image will match the reference letter(100-n)%

So, if we assume 10% noise (10% n), then probability of pixels matching is 0.9 which can be written as:  
```
P(all observed pixels| reference letter) = (0.9) ^ (no. of matched pixels) * (0.1) ^ (no. of unmatched pixels)
```

As we saw earlier while solving Part 1 for "Simplified Model", each observation is only dependent upon its state.
Thus,
```
s_i = argmax {si}P(Si = si|W) which is proportional to argmax{si} P(W|Si=si)
```

In this method, (HMM) before implementing variable elimination, we have used viterbi algorithm. Later, viterbi algorithm can be converted to variable elimiation by swapping the max with sum.

### Sample Output
```
zsh> /opt/homebrew/bin/python3 /Users/satwik/git/EAI/hipatil-mbirari-satkulk-a3/Part2/image2text.py Part2/test_images/courier-train.png Part2/test_images/12345.txt Part2/test_images/test-0-0.png
(1008, 25)
1008
(477, 25)
476
Simple: SUPREME COURT OF THF UN1TED STATES
HMM VE: SUPREME COURT OF THF UN1TED STATES
HMM MAP: SUPREME COURT OF THE UN1TED STATES
```
