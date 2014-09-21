Sentiment-Analysis
==================

Sentiment analysis for the movie reviews- whether the movie review is positive or negative

//Below is the code:

from collections import Counter
import operator

#Load positive DataSet
def load_positive():
    f = open("E:\\Sofia_Assignment\\Train-Data-Positive.txt")
    positive_lines = []
    for each in f.readlines():
        each_line = each.strip()
        positive_lines.append(each_line)
    print len(positive_lines)
    return positive_lines

    
#Load negatice Dataset
def load_negative():
    f = open("E:\\Sofia_Assignment\\Train-Data-Negative.txt")
    negative_lines = []
    for each in f.readlines():
        each_line = each.strip()
        negative_lines.append(each_line)
    print len(negative_lines)    
    return negative_lines

positive_lines = load_positive()
negative_lines = load_negative()


positive_words = Counter()
all_positive_words = []
for each_positive_line in positive_lines:
    for each in each_positive_line.split():
        positive_words[each] += 1
        all_positive_words.append(each)

print len(positive_words)
print len(all_positive_words)

negative_words = Counter()
all_negative_words = []
for each_negative_line in negative_lines:
     for each in each_negative_line.split():
        negative_words[each] += 1
        all_negative_words.append(each)

print len(negative_words)
print len(all_negative_words)


vocab_count = len(set(positive_words.keys() + negative_words.keys()))
print vocab_count

from decimal import *
import math
getcontext().prec = 6
probability = 0.0
#Compute probability
P_C = math.log((len(positive_lines) / float(len(positive_lines) + len(negative_lines))))
P_J = math.log(len(negative_lines) / float(len(positive_lines) + len(negative_lines)))



#Compute Conditional probability

def compute_probablity(word, classifier):
    #Word from test data
    #Classifier to which we need to run it on.
    if classifier == 'C':
        count_of_words = positive_words[word]        
        #print count_of_words
        count_positive_words = len(all_positive_words)        
        probability = (count_of_words + 1) / Decimal(count_positive_words + vocab_count)
        #print probability
        #return float("{0:.3f}".format(probability))
        
        return math.log(probability)
    elif classifier == 'J':
        count_of_words = negative_words[word]        
        #print count_of_words
        count_negative_words = len(all_negative_words)        
        probability = (count_of_words + 1) / Decimal(count_negative_words + vocab_count)
        #print probability
        #return float("{0:.3f}".format(probability))
        return math.log(probability)



#File for test data:
test_data = open("E:\\Sofia_Assignment\\Test-Data.txt")
for each_test_line in test_data.readlines():
    pos_results = []
    neg_results = []
    each_test_line = each_test_line.replace("<br /><br />","")
    for each_word in each_test_line.split():
        #print each_word
        res_pos = compute_probablity(each_word,'C')
        #if res_pos > 0.0:
        pos_results.append(res_pos)
        res_neg = compute_probablity(each_word,'J')
         #if res_neg > 0.0:
        neg_results.append(res_neg)

    x = 0.0
    for each in pos_results:
        #print x
        x += each    


    each = 0
    y = 0.0
    for each in neg_results:
        #print x
        y += each    
    if x > y:
        print "Positive"
    else:
        print "Negative"
