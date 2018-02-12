---
layout: page
title: Solving the cd-hit -d flag problem
comments: true
---

When you do `cd-hit` to filter for identical or near identical sequences in any bioinformatics pipeline, you get a `.clstr` file. Sometimes you need to parse that file for getting the sequences in the individual clusters. By default, in the cd-hit command, the `-d` flag is set to 0, which means you can only see the fasta sequence identifier until you hit a whitespace in your `.clstr` file. By putting in your preferred number with the `-d` flag you can extend the identifier to that many characters.

As of, May 5, 2016, the master branch of cd-hit in the github [repo](https://github.com/weizhongli/cdhit/) does not conform to the `-d` flag. As I needed the full fasta record description for each sequence in a cluster, this was indeed a headache. So, I searched for this problem. There was not much but an [issue](https://github.com/weizhongli/cdhit/issues/4) was indeed opened regarding the same problem in the github repo of cd-hit.

I found a solution there which involves changing one line in the `cdhit-common.c++` file. You can find it in your `cdhit-master` directory which is basically the master branch of cd-hit in github. There, in the `void SequenceDB::Read` function you have the following lines -

```cpp
int i = 0;
if( des.data[i] == '>' || des.data[i] == '@' || des.data[i] == '+' ) i += 1;
if( des.data[i] == ' ' or des.data[i] == '\t' ) i += 1;
if( options.des_len and options.des_len < des.size ) des.size = options.des_len;
while( i < des.size and ! isspace( des.data[i] ) ) i += 1;
des.data[i] = 0;
one.identifier = dummy.identifier = des.data;
} else {
    one += buffer;
}
```

Here, you need to change the 5th line which is a while loop. Delete that and insert the following line there -

```cpp
while( i < des.size and ( des.data[i] != '\n') ) i += 1;  
```

Now, with the `-d` flag you can put the number as you wish and in the `.clstr` file you should get the fasta sequence identifiers up to that number. Here is a portion my `.clstr` file with the `-d 300` flag -

    >Cluster 0
    0       702aa, >32.2 | product: bacteriocin immunity protein | note: no note | protein_id: CJQ31854.1 | CLSP01000002:19049-21157 | immunity... *
    1       702aa, >34.2 | product: bacteriocin immunity protein | note: no note | protein_id: CJV34097.1 | CMAH01000025:21097-23205 | immunity... at 99.57%
    2       702aa, >27.2 | product: bacteriocin immunity protein | note: no note | protein_id: COH18396.1 | CRGD01000006:80660-82768 | immunity... at 99.43%
    3       702aa, >27.2 | product: bacteriocin immunity protein | note: no note | protein_id: COI25170.1 | CRIA01000001:244000-246108 | immunity... at 99.86%
    4       702aa, >28.2 | product: bacteriocin immunity protein | note: no note | protein_id: COH18396.1 | CRGD01000006:80660-82768 | immunity... at 99.43%
    5       702aa, >28.2 | product: bacteriocin immunity protein | note: no note | protein_id: CJN26788.1 | CLNU01000003:29923-32031 | immunity... at 99.72%
    6       702aa, >28.2 | product: bacteriocin immunity protein | note: no note | protein_id: COI25170.1 | CRIA01000001:244000-246108 | immunity... at 99.86%
    7       702aa, >28.2 | product: bacteriocin immunity protein | note: no note | protein_id: COJ04657.1 | CRJL01000001:149015-151123 | immunity... at 99.72%
    8       702aa, >28.2 | product: bacteriocin immunity protein | note: no note | protein_id: COM73718.1 | CROP01000007:61006-63114 | immunity... at 99.72%
    9       702aa, >39.2 | product: bacteriocin immunity protein | note: no note | protein_id: COA67976.1 | CHPO01000002:36267-38375 | immunity... at 97.58%
    10      702aa, >36.2 | product: bacteriocin immunity protein | note: no note | protein_id: CJQ31854.1 | CLSP01000002:19049-21157 | immunity... at 100.00%
    11      702aa, >35.2 | product: bacteriocin immunity protein | note: no note | protein_id: CIS40931.1 | CHNO01000004:74173-76281 | immunity... at 99.72%
    12      702aa, >30.2 | product: bacteriocin immunity protein | note: no note | protein_id: CIS40931.1 | CHNO01000004:74173-76281 | immunity... at 99.72%
    13      702aa, >30.2 | product: bacteriocin immunity protein | note: no note | protein_id: CJV34097.1 | CMAH01000025:21097-23205 | immunity... at 99.57%


Before, I was only getting up to the first whitespace in these seq descriptions like `0     702aa, >32.2...`. Hope that helps you save some precious time while hammering your head against the wall.
