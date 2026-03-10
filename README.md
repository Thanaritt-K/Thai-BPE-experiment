# Examination of Byte Pair Encoding (BPE) for Thai language
## Information  
Thanaritt Kunajak  
Natural Language Processing  
Master’s in Theoretical and Applied Linguistics (8037)  
Universitat Pompeu Fabra  
Exercise 1



## 1 Introduction

Subword segmentation is widely used to address the open vocabulary problem in machine translation (Bojar et al., 2018) and Byte Pair Encoding (BPE) (Sennrich et al., 2016) is the dominant approach to subword segmentation. Based on a compression algorithm (Gage,1994), it keeps the common words intact while splitting the rare and unknown ones into a sequence of subword units. This potentially allows a model to make use of morphology, word composition and transliteration. BPE effectively deals with an open vocabulary problem and is widely used due to its simplicity. Furthermore, the main difference to other compression algorithms is that the compressed symbol sequences are still interpretable as subword units, which will allow us to examine how accurate this subword segmentation is compared to our knowledge of Thai morphemes.

## 2 Material and methods

There is a major issue when training the BPE algorithm for Thai, as Thai has no clear word boundaries, which are required for BPE to learn its vocabulary. Thai word segmentation is complicated by linguistic ambiguity and out­ of ­vocabulary cases (Chormai et al., 2020). A word can be formed by juxtaposing two “words” e.g. เห็นชอบ (/hen tɕʰɔ:p/, approve) = เห็น (/hen/, see) + ชอบ (/tɕʰɔ:p/, like). This kind of word formation can be detected with a simple dictionary lookup, but harder cases that require context abound in the language. One classic example is ตากลม, which can be segmented into ตา|กลม (/taːk lom/, round eye) or ตาก|ลม(/taː klom/, be exposed to the wind/fresh air).

### 2.1 Data Sources

For this experiment, we will use a manually tokenized corpus, BEST-­2010 (Krit Kosawat et al., 2009; NECTEC, 2010). Annotated with word boundaries and name entities, the corpus contains 415 Thai documents from four categories: news, articles, encyclopedias, and novels, accounting for 134,107 samples (split by line). The data is available for public use and could be found at github.com/korakot/corpus/tree/main/BEST. (Also available at: https://huggingface.co/datasets/nectec/best2009) This corpus selection is to ensure that we are not introducing errors via the training data’s tokenization method, while ensuring that the texts are of natural language.

### 2.2 Preprocessing steps

With the available word boundaries annotation, the tokenization process is quite simplistic by replacing the word boundaries (“|”) with a whitespace. But to preserve the original whitespaces, treating them as part of the symbols for BPE, and to keep the information lossless, we will have to first escape whitespaces with a meta symbol _ (U+2581)(Kudo & Richardson, 2018). After that, tags and some punctuation marks, such as quotation marks and parentheses, were removed.

### 2.3 Evaluation Metrics 

As far as I know, there’s currently no study on automatic morphemic analysis/segmentation in Thai (as seen in the most used Thai NLP library (Wannaphong Phatthiyaphaibun et al., 2023): https://github.com/PyThaiNLP/pythainlp/tree/dev/pythainlp/morpheme), nor is there any corpus available with morpheme annotations. Hence, we could only rely on some manually created toy examples for accuracy-based comparison. But to make the comparison meaningful and interesting, we’ll compare subword segmentation results from BPE with other relevant subword segmentation methodology, namely, syllable-based tokenization based on trigram statistics of syllables.(https://pypi.org/project/tltk/)(Aroonmanakun, 2002)

With the low amount of manually created test sentences, I don’t think any kind of statistical metrics would be meaningful, so I decided to not do this, and only review segmentation results to showcase them.

## 3 Results

For BPE I’ve experimented with 3 level of vocabulary size: 500, 3000, and 5000. (Similar to previous work in Heinzerling & Strube, 2018) The results have shown some semblances to the morphemes.

While syllable-based is, as the name suggested, tokenizing the utterance based on the syllable boundaries, which occasionally coincides with morphemic boundaries.  This could leads to overtokenization, nonetheless, as although Thai is commonly thought to be a monosyllabic language like Chinese, many words are disyllabic and trisyllabic. Most disyllabic words are names of plants and animals. Others are Khmer-related.(Siripong Potisuk, 2009)

I believe that BPE would lead to interesting applications of morphemic analysis, with the right hyperparameter and training data, as currently, lots of analysis are based on purely diachronic corpora, or language contact data, which might not be available for low resource languages, such as Thai.

## 4 Code

The code to the experiment is available online at: https://github.com/Thanaritt-K/Thai-BPE-experiment
