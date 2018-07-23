Machine Learning Algorithms for Chinese Word Segmentation

presentation script:

In some languages like Chinese, there is no word boundary markers like space in English. For humans, if you want to split up a Chinese sentence by words, you have to know the semantic meanings, sometimes you also need some help from the context. Therefore, it’s very hard for machines to understand how to parse Chinese articles and extract information from them. This technique is very important for like refining search engine results, filtering email by categories by contents, censoring webpage content in Chinese and other similar languages.
Currently there are 3 types of algorithms for machines to do Chinese word segmentation. The first type is to use neural network and expert systems to train AIs to parse Chinese like humans do. It may be the best way to parse Chinese, but, as far as I know, currently we can’t do this. 
The second type of algorithms are dictionary-dependent. For example, forward procedure. For example, “Furman University has a bell tower”. First, we cut the sentence in half. Let’s analyze the second half of the sentence as an example (I’ll explain later why I chose the second half). Then we ask whether the current sequence of characters is a legit word in the dictionary. If not, chop down the last character. The operations we perform on the second half of this sentence would be the following:
“有一座钟塔”  not a word. Chop. \n
“有一座钟”  not a word. Chop. \n
“有一座”  not a word. Chop.
“有一”  not a word. Chop.
“有”  “have/has” is a word. Add the word to the sentence.
“一座钟塔”  not a word. Chop.
“一座钟”  not a word. Chop.
“一座”  “a/an” is a word. Add the word to the sentence.
“钟塔”  “bell tower” is a word (these two words in English can be considered as one word in Chinese because it’s in the dictionary).
“” Empty string. Stop.
 
However, this algorithm depends too much on the dictionary, the dictionary should be constantly updated for like borrowed words. It’s hard to parse something like names (for example, the word “Furman” in the first half of the sentence) that is not in the dictionary. Also, it’s not good at dealing with ambiguities, like when a character can go with either the previous character or the latter character and both can form a word. Sometimes, one word has more than one meanings. Machines using dictionary dependent algorithms cannot distinguish between multi-meanings.
The third type of algorithms are machine learning algorithms. Algorithms in this category requires a large amount of known correct segmentation data to train the machines and then machines can segment new articles by probabilities of the appearance of the words. Hidden Markov Model is a general algorithm for Chinese word segmentation. It is based on Bayesian model and Viterbi algorithm and its structure it’s like a linked list. One assumption in HMM is that each state is only related to the previous state. That’s not always the case in language! For example, this phrase can be “older student”(大/学生) or “college student”(大学/生) based on different segmentation. You need some context to determine which one it actually is. And actually, once the second character in this phrase is consumed, it changes the probability of the following segments. However, HMM doesn’t respond to this change.
Then we have a better algorithm that does consider the consumption of a character changes the probability of the following segments, Conditional Random Field. In the following example, the first character can be a word alone, meaning “in the middle”; it can be combined with the second character, meaning China; adding the third character to it, it means “Chinese” referring to people. 
 
In this algorithm, you first assign a label to each of the characters when reading the inputs. For example, a common label system is “BMES” stands for beginning, middle, end and single-character word. In this example, we have:
 .
 then you must choose a template to assist the probability of each different segmentation. There are many different templates, this is only one of the easiest. 
 
Suppose we are reading the 3rd character “人”. X[0, 0] would be “人”, X[-2, 0] would be “中”, X[-1, 0] would be “国” and so on. “U00” of the template would be the probability of the single character word “中”, “U05” would be the probability of the trigram “中国人” and so on. 
