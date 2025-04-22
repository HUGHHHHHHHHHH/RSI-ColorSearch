# A GUIDED COLOR SEARCH PROJECT
This is a dataset of a large-scale online experiment on guided color search. The experiment was conducted by Mr. Yuxuan Zheng of The Chinese University of Hong Kong during January and Febuary of 2024. The dataset is shared for the purpose of Research Science Institute training only. Please contact Mr. Zheng (zhyx@link.cuhk.edu.hk) for any inquiry.

### Experiment Task
Participants will first be shown the template of target color in the middle for 1s, and later given 10s to search on the panel to see whether the target exists. Participants press the target if target exists, and press the target template if target does not exist. See Task.png. There are 32 possible slots, and 35 possible colors. 312,000 task patterns are generated.

All the task patterns are stored in idx_pattern_map.csv. The first 32 columns represent the color_id of item on each slot (0 if no item). Col 33 is the target position (0 if absent), and Col 34 is the target color_id. In .\pics, you can find the corresponding color of each color_id. 

### Data Structure
The original raw data is in raw_data.xlsx. Col *ID* represents the unique ID of each player; col *save_data* records the data blocks of 50 consecutive responses. Each response's pattern, choice (1~32: slot no., 0: target template; -1: overtime) and response time in ms are recorded.
**corrent_data.csv** parses all valid correct responses. The data sturcture is like:
| 0 (Pattern index) | 1 (Selection) |2 (RT) |3 (Class)|
|-------------------|---------------|-------|---------|
|260159: sample pt_i| 21: 21st slot |1620 ms|   5-3   |

Class A-B means there are min(2^A, 24) items and min(2^B, 24) possible different distractor colors. Class corresponds to Pattern index in the following rule:
|-|C1|C2|C4|C8|C16|C24|
|-|-|-|-|-|-|-|
|I1|1||||||
|I2|6001|14001|||||
|I4|24001|34001|46001||||
|I8|60001|72001|86001|102001|||
|I16|120001|134001|150001|168001|188001||
|I24|210001|222001|236001|252001|270001|290001|

(I1, C1) = 1 means 1-item, 1-distractor color indices start from 1 (and ends at 6000, because the next class starts from 6001).
**faulty_data.csv** aprses all valid incorrect responses. The data structure is like:
| 0 (Pattern index) | 1 (Selection) |2 (RT) |3 (null)|
|-------------------|---------------|-------|--------|
|260159: sample pt_i|  0: template  |2101 ms|    0   |

### Potential Research Questions
1. Participants make mistakes due to various reasons, but some patterns record more faulty responses than others. What factors (e.g., colors and positions of targets and distractors) are related to faulty rate (the proportion of faulty responses of a pattern)? Hint: you may start by training deep neural networks with different inputs to see which factors are important.
2. Some patterns take longer time for participants to decide, partly because the pattern contains similar colors, and it takes time to differentiate them and make decisions. Try to design a color grouping algorithm to group similar colors, and use the grouping results (e.g., # of distractors that are *similar* to target color), and examine your answer against the response time. Hint: since response time could also be dependent on item count, distractor color count and target presence/absence, you may want to do the correlation/regression analysis against certain groups of patterns. However, it could be lovely if you find a universal rule.
