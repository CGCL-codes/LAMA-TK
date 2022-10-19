
# LAMA-TK  
  
This folder contains the LAMA-TK dataset. The dataset is described in this paper **Can Language Models Serve as Temporal Knowledge Bases?**  You can directly download the preprocessed data at:  
> https://drive.google.com/file/d/1lwcPi-ExuMAtUBmvXY5dL0Lkg6VgiC4f/view?usp=sharing
  
## Knowledge Sources
The knowledge sources of LAMA-TK are from [CronQuestions](https://github.com/apoorvumang/CronKGQA) and [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page). We extracted all facts with both a start date and an end date. We reserved the top 7 most frequent temporally rich relations from our collected temporal knowledge, namely
$\bullet$ position held
$\bullet$ member of sports team
$\bullet$ employer
$\bullet$ award received
 $\bullet$ country of citizenship
 $\bullet$ spouse
This results in 639k temporally-scoped facts, with 316k entities. 
## Fact to Text 
For each fact, we write a template to convert it to natural language statements. For example, for the fact <Giannis Antetokounmpo, Member of Sports Team, Filathlitikos B.C., 2011, 2013>, we use the template "[X] held the position of [Y]" to convert the fact triple <Giannis Antetokounmpo, Member of Sports Team, Filathlitikos B.C.> to text "Giannis Antetokounmpo played for Filathlitikos B.C.". 

For most relations, we use the prompt template "from ST to ET" to convert temporal scopes to natural language texts. However, "award received" is an exception. It is not a durative relation, the start time of the facts is always equal to the end time. Therefore, we use a new prompt template "in T" to convert these temporal scopes to texts. Finally, we concatenate the factual text and temporal text, and we get the factual statements.

## Constructing Queries
For each factual statement, we mask the subject, object, start time, and end time, resulting in four masked statements. We reserve the masked entity and collect all correct answers to the masked statements as the answer list. We train the LMs to predict the masked entity, and use both the masked entity and the answer list for evaluation purposes. If the masked entity is in the top k answers of the model, Hit@K is 1. If any of the top k answers of the model is in the answer list, Acc@K is 1.

Note that previous works tended to mask the object of each factual statement only because masking the subject would introduce multi-answer statements. For example, "[MASK] played for Chicago Bulls from 1995 to 1998" has more correct answers than "Michael Jordan played for [MASK] from 1995 to 1998". 
# Support or Contact  
LAMA-TK is developed at National Engineering Research Center for Big Data Technology and System,  Services Computing Technology and System Lab, Cluster and Grid Computing Lab, School of Computer Science and Technology, Huazhong University of Science and Technology, China(http://grid.hust.edu.cn/) and Data Science and Machine Intelligence Lab, University of Technology Sydney, Sydney, Australia by Ruilin Zhao, Feng Zhao, Guandong Xu , and Sixiao Zhang. For any questions, please contact Ruilin Zhao(ruilinzhao@hust.edu.cn), Feng Zhao(zhaof@hust.edu.cn), Guandong Xu(guandong.xu@uts.edu.au) and Sixiao Zhang(zsx57575@gmail.com)
