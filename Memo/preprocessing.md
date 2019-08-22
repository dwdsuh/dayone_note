---
title: "[Python] Preprocessing \"Big Data\" / 대용량 파일 전처리"
date: 2019-07-14T16:31:46+09:00
draft: false
---

### [Preprocessing]: How to Randomly Sample from a File of Tremendous Size.


![big data image](https://media.makeameme.org/created/big-data-big-5ab38c.jpg)




Dealing with Big Data, I usually have to handle data "bigger" than 10GB. Last week, I wanted to sample 100K sentences from the file of 50M-ish sentences. At first, I naively wrote a code to open the file and select samples using indexing samples with numpy.random. Unfortunately, the code was starting to deprive me of my lifetime. I cannot help interrupting the code and searching for the faster method. 

Thanks to the [Massould's wonderful code](http://metadatascience.com/2014/02/27/random-sampling-from-very-large-files/), I've successfully randomly sampled sentences in a few seconds.

Python Code:

```python
def sampling_from_file_faster(filepath, sample_num):
    sentence_list=[]
    with open(filepath, "rb") as f:
        f.seek(0,2)
        filesize=f.tell()
        
        sample_index=sorted(random.sample(range(filesize), sample_num))
        
        for i in range(sample_num):
            f.seek(sample_index[i])
            f.readline() #skip the current line
            sentence_list.append(f.readline().rstrip().decode("utf-8"))
            print("Preprocessing: %0.2f percent completed"%(i/sample_num*100), end='\r')
    return sentence_list
```

Explanation for Each Line

+ Opening the file in Binary Mode

```python
with open(filepath, "rb") as f:
```
> Using option "rb" saves a lot of time. Binary Code can be converted into "human-friendly letter" with the method .decode("utf-8"). 


+ Setting the pointer
```python
f.seek(0,2)
```
> f.seek(0,2) seeks for the file end, the process of which is necessary to measure the file size. 


+ Measuring the file size
```python
filesize=f.tell()
```
> f.tell() returns an integer that tell the position of the file end in byte-wise.


+ Sampling the indices of sample
```python
sample_index=sorted(random.sample(range(filesize), sample_num))
```
> Selecting the sample by the byte-wise position.


+ Get samples by the indices
```python
f.seek(sample_index[i])
f.readline()
sentence_list.append(f.readline().rstrip().decode("utf-8"))
```
> First, find the sentence that include the byte of the sample index. Second, skip the current line since the index are likely to refer to the middle of a sentence. Third, append the next sentence to the pre-declared list, converting binary code to utf-8 at the same time. 

**Now we have a fancy method that can sample sentence from tremendous-size file in a few seconds.**


