---
layout: post
title: "C ++ algorithmic challenges"
date: 2014-05-19 18:41:00
---

_Originally posted on my old blog on 2014-05-19._

###  Pogramming 

##  mY COde REpository 

  
**__some of th algorithmic problems which i find interesting i have written a kind of tutorial for them here .__**  
  
  
[**__(1).Stable Marriage Problem__**](http://en.wikipedia.org/wiki/Stable_marriage_problem)  
_In[mathematics](http://en.wikipedia.org/wiki/Mathematics "Mathematics") and [computer science](http://en.wikipedia.org/wiki/Computer_science "Computer science"), the **stable marriage problem** (**SMP**) is the problem of finding a **stable matching** between two sets of elements given a set of preferences for each element. A [matching](http://en.wikipedia.org/wiki/Matching_%28graph_theory%29 "Matching \(graph theory\)") is a mapping from the elements of one set to the elements of the other set. A matching is stable whenever it is not the case that both:_  
  


  1. _some given element A of the first matched set prefers some given element B of the second matched set over the element to which A is already matched, and_
  2. _B also prefers A over the element to which B is already matched_

_In other words, a matching is stable when there does not exist any alternative pairing (A, B) in which both A and B are individually better off than they would be with the element to which they are currently matched._  
_The stable marriage problem is commonly stated as:_  


    _Given n men and n women, where each person has ranked all members of the opposite sex with a unique number between 1 and n in order of preference,[marry](http://en.wikipedia.org/wiki/Marriage "Marriage") the men and women together such that there are no two people of opposite sex who would both rather have each other than their current partners. If there are no such people, all the marriages are "stable"._
    
    
    The code I had written is a nice implementation of algorithm given in Wiki.The code is wri
    
    
    tten in C++ .Its does not use much Stl ,as a result ,it is fast and efficient and is tested
    
    
    on the following problems::
    
    
    <http://www.codechef.com/problems/STABLEMP/>  
    
    
    [https://www.spoj.pl/problems/STABLEMP/ ](https://www.spoj.pl/problems/STABLEMP/)
    
    
    For n=500 and 100 test cases ,the execution time as per [spoj](https://www.spoj.pl/status/STABLEMP,ritesh_gupta/) is 0.49 seconds .
    
    
    
    
    
    #include <iostream>
    #include <stdio.h>
    #include <cstdio>
    #include <stack>
    #define MAX 510
    using namespace std;
    int Women_List[MAX][MAX],Man_List [MAX][MAX];
    int preferences[MAX][MAX];
    int womanEngagedto[MAX],manEngagedto[MAX];
    int ManhastoPropose[MAX];
    
    void stableMarriage(int N)
    {
     stackfreeMan;
     int m,w,woman_index_number,m1;
    
     //Initialisation Phase
     //Initialize all m ? M and w ? W to free
     for(int i=1;i<=N;++i)
     {
      freeMan.push(i);  //Man Number I is Free
      womanEngagedto[i]=0;    //Initially all Women are Free .So womanEngagedto[i]=0 :: i th woman is Free
      ManhastoPropose[i]=1; //At the Beginning ,the man has not propose any woman in his preference list.So index set to 1 ,ie. Man has to propose number 1 woman.
     }
    
     //while there exists free man m who still has a woman w to propose to
     while(!freeMan.empty())
     {
    
      m=freeMan.top();      //m is the free man .
      woman_index_number=ManhastoPropose[m];  //Man has not proposed woman_index_number indexed woman in his list
      ManhastoPropose[m]+=1;     //The man m  has proposed the  Man_list[woman_index_number] now .So next time
      //the man m has to propose Man_list[woman_index_number+1].
    
      w=Man_List[m][woman_index_number];     //w = m's highest ranked such woman to whom he has not yet proposed
    
      //if w is free (m, w) become engaged
      if(womanEngagedto[w]==0)      //womanEngagedto=0 :: Means she is Free.
      {
       womanEngagedto[w]=m;      //The womanNumber W is now  engaged to man m;
       manEngagedto[m]=w;
       freeMan.pop();        //The Free Man M is now Engaged ,so lets pop him from stack
       continue;         //Boyzz - Man is popped ,is engaged now ,so why to go further.
      }
      else                         //Else some pair (m', w) already exists
      {
    
       m1=womanEngagedto[w];             //The woman w is not free and is already engaged to man m1 :( sad for Man  m
    
       // But Man m has still a chance ,Lets check whom woman w Prefers a lot.
    
       if(preferences[w][m]>preferences[w][m1])//if woman w prefers m to m1 (ie prefw for m >pref w for m1)
       { 
        freeMan.pop();     //Then Woman w will be engaged with Man m.Man M is done,so lets pop him from stack.
        womanEngagedto[w]=m;   //(m, w) become engaged 
        manEngagedto[m]=w;
        freeMan.push(m1);    //Man m1 is now free .Let him search for another woman by pushing him to stack.
       }
       else
       {
    
        /*This  is important . The wikipaedia psudo code doesnot explain this properly*/
        /*if preference of man m is lower than m1 ,then women will stick to the partner
        (m1, w) remain engaged.
        But Man 'm' is still unengaged .
        So what to do now????
        Note for this reason i had not popped man m after m=freeMan.top() statement.
        Solution::
        Under that case ,the man m searches the next high ranked such woman to whom he has not yet proposed
        */
    
       }
      }
     }
     for(int i=1;i<=N;++i)
      printf("%d %d\n",i,manEngagedto[i]);
    }
    
    int main()
    {
     //freopen("in.txt","r",stdin);
     //freopen("out.txt","w",stdout);
     int T,N,neglect,prefOrder;       
     scanf("%d",&T);     //Enter Test Cases
     while(T--)
     {
      scanf("%d",&N);    //Number of Total Marriage to find
    
      //Entry of Woman Preference List
      for(int i=1;i<=N;++i)
      {
       prefOrder=1<<30;
       scanf("%d",&neglect);
       for(int j=1;j<=N;++j)
       {
        scanf("%d",&Women_List[i][j]);     // The woman number i , jth preference is Women_List[i][j] man
        preferences[i][ Women_List[i][j] ]=prefOrder--;
       }
      }
    
      //Entry of Man Prefernces List
      for(int i=1;i<=N;++i)
      {
       scanf("%d",&neglect);
       for(int j=1;j<=N;++j)
       {
        scanf("%d",&Man_List[i][j]);  // The man number i , jth preference is Man_List[i][j] woman
       }
      }
    
      stableMarriage(N);
     }
     return 0;
    }
    
    
    
    
    
     

#  [Knuth–Morris–Pratt algorithm](http://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm)

In computer science, the Knuth–Morris–Pratt string searching algorithm (or KMP algorithm) searches for occurrences of a "word" W within a main "text string" S by employing the observation that when a mismatch occurs, the word itself embodies sufficient information to determine where the next match could begin, thus bypassing re-examination of previously matched characters.  
The Simplest and the most common application of KMP is ,finding a pattern in a large text.  
_Instead of the common problems , I will discuss how KMP can be modify to solve various string manipulation problems._  
_[KMP : Application 1 - Extend the given string to Palindrome](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2470)  
_  
Given a word, append the fewest number of letters to it to convert it into a palindrome.  
The approach will be that we want to find how many of the characters at the end of the string can be reused when appending the extra characters to complete the palindrome.  
Using KMP, we search the original string for its reverse. Once we get to the very end of the string, we will have as much a match as possible between the reverse of the string and the original string that occurs at the end of the string.  
The overall complexity= runtime of a standard KMP search plus the time required to reverse the string: O(n) + O(n) = O(n).  
  
  

    
    
    #include <iostream>
    #include <stdio.h>
    #include <cstdio>
    #include <stack>
    #include <algorithm>
    #include <string.h>
    #define MAX 1000001
    using namespace std;
    char text[MAX];
    int F[MAX];
    int noofmatches=0;
    int pl,tl,textptr=0,patternptr=0;
    int matched_up2=0;
    void KMP_FAILURE(char *pattern)
    {
     int i,j;
     F[0]=F[1]=0;
     for(i=2;i<=pl;++i)
     {
      j=F[i-1];
      for(;;)
      {
       if(pattern[j]==pattern[i-1])
       {
        F[i]=j+1;
        break;
       }
       if(j==0)
       {
        F[i]=0;
        break;
       }
       else
        j=F[j];
      }
     }
    }
    void kmp(char *pattern)
    {
    
     textptr=0,patternptr=0;
     for(;textptr<=tl;)
     {
      
      if(text[textptr]==pattern[patternptr])
      {
       matched_up2=patternptr;
       textptr=textptr+1;
       patternptr=patternptr+1;
       if(patternptr==pl)
       {
        noofmatches+=1;
       }
    
      }
    
      else if(patternptr>0)                  
      {
       patternptr=F[patternptr];
      }
    
      else
      {
       textptr+=1;
      }
     }
    
    }
    int main()
    {
     int i;
     while(gets(text))
     {
      char pattern[100001];
      matched_up2=0;
      strcpy(pattern,text);
      reverse(pattern,pattern+strlen(pattern));
      tl=strlen(text);
      pl=strlen(pattern);
      KMP_FAILURE(pattern);
      kmp(pattern);
      //printf("matched_up2 %d\n",matched_up2);
      printf("%s",text);
      if(matched_up2>=pl)
      {
       printf("\n");
      }
      else
      printf("%s\n",pattern+(matched_up2+1));
     }
     return 0;
    
    }
    
    

  
  
  
  
**_Matrix Exponentiation and Recurrence Relation Solving Technique_**  
  

    
    
    #include <stdio.h>
    #include <stdlib.h>
    #define  Mod 1000000007
    #define  K 2
    typedef long long int Int_64;
    /*******************************************************************************************************************************/
    //K is Dim
    //Fn[i]=coeff_1*Fn(i-alpha)+coeff_2*Fn(i-Beta)+...+coeff_KFn(i-Zeta)
    /******************************************************************************************************************************/
    
    /*In Matrix Exponentiation All Arrays are 1 Based.
    Function That Multiply the Two Matrices Matrix1 * Matrix 2
    */
    
    //Variables
    Int_64   **productMatrix;
    Int_64   F1[K+1];
    Int_64   **X;
    Int_64   **T;
    
    // Returns the Simple Matrix Multiplication of Two Matrices M1 and M2.
    Int_64** Multiply(Int_64 **M1,Int_64 **M2)
    {
     int i,j,k;
     productMatrix=(Int_64**)malloc((K+2) * sizeof(Int_64*));
     for(i=0;i<=K;++i)
      productMatrix[i]=(Int_64*)malloc((K+1) *sizeof(Int_64));
    
     for(i=1;i<=K;++i)
     {
      for(j=1;j<=K;++j)
      {
       productMatrix[i][j]=0;
       for(k=1;k<=K;++k)
       {
        productMatrix[i][j]=( productMatrix[i][j]+(M1[i][k]*M2[k][j]) ) % Mod;
       }
      }
     }
     return productMatrix;
    }
    /*
    To solve    :(Matrix)^P ;
    The Following is Logn solution.
    
    |Return  Matrix                      ->if P==1;
    (Matrix^P)=|Return  (Matrix)*(Matrix^(P-1))     ->if P is Odd
    |Return  (Matrix^P/2)*(Matrix^P/2)   ->if P is Even
    */
    Int_64** Power(Int_64 **A,Int_64 p)
    {
     if (p == 1)
      return A;
     if (p % 2)
      return  Multiply(A, Power(A, p-1));
     X = Power(A, p/2);
     return  Multiply(X, X);
    }
    Int_64 solve(Int_64 N)
    {
     int i;
     Int_64 result=0;
     /* 
     Example consider the Recurrence Relation to be 
     Fn(N)=(2*Fn(N-1)+2*Fn(N-2))
     Fn(1)=1,Fn(K=2)=3;
    
     Step 1:
     As Every DP starts with a base case ,so Similar is the Case Here
     Base case is a column Vector F1
     Eg .When FN[i]=FN[i-1]+4*FN[i-2]+6*FN[i-3]+8*FN[i-4]
    
     |FN(0)=NULL|
     |FN(1)     |       
     F1:|FN(2)     |
     |FN(3)     |
     |FN(K=4)   |(K*1 Dimension)
     */
     F1[0]=1;
     F1[1]=1;
     F1[K]=3;
    
     /*Step 2: Fill the T Matrix
     Eg:FN[i]=FN[i-1]+4*FN[i-2]+6*FN[i-3]+8*FN[i-4];
    
     |N N N N N |
     |N 0 1 0 0 |
     T(K*K)= |N 0 0 1 0 |
     |N 0 0 0 1 |
     |N 8 6 4 1 |        
     */
     T=(Int_64**)malloc((K+2) *sizeof(Int_64*));
     for(i=0;i<=K;++i)
      T[i]=(Int_64*)malloc((K+1) *sizeof(Int_64));
    
    
     T[1][1]=0,T[1][K]=1;
     T[2][1]=2,T[2][K]=2;
    
     /*Step 3:
     Apply :: T=(T^N-1)
     */
     if(N==1)
      return F1[1];
    
     T=Power(T,N-1);
    
     /*Step 4:
     So the Overall Answer is::
     First row of (T*F1).
     */
     result=0;
     for(i=1;i<=K;++i)
     {
      result=(result+T[1][i]*F1[i])%Mod;
     }
     return result;
    
    }
    int main()
    {
     int TestCases;
     Int_64 N;
     scanf("%d",&TestCases);
     while(TestCases--)
     {
      scanf("%lld",&N);
      printf("%lld\n",solve(N));
     }
     return 0;
    }
    
    
    

  
  
__Edmond Karps -Network Flow__  
  

    
    
    /*Tested at http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=9
    */
    #include <algorithm>
    #include <iostream>
    #include <stdio.h>
    #include <queue>
    #define MAX 1000
    #define NIL -1
    using namespace std;
    
    int Graph[MAX][MAX],flow[MAX][MAX],ResidualGraph[MAX][MAX];
    int Totaledges,TotalVertices;
    int color[MAX],parent[MAX],Distance[MAX];
    vector<int>path;
    void print_path(int s,int v)
    {
     if(v==s)
      path.push_back(s);
     else if(Distance[v]==NIL)
     {}
     else
     {
      print_path(s,parent[v]);
      path.push_back(v);
     }
    }
    bool bfs(int source,int sink)
    {
     int i,j,u,v;
     const int WHITE=0,GRAY=1,BLACK=2;
     queue<int>Queue;
     //Initialise all the vertices to white
     for(i=0;i< TotalVertices+1;++i)
     {
      color[i]=WHITE;          //Initially all the vertices are undiscovered
      parent[i]=NIL;
      Distance[i]=MAX;
     }
    
     //Enque the Start Vertex(Source) and Paint it Gray
     Queue.push(source);
     color[source]=GRAY;
     Distance[source]=0;
    
     //Until queue is empty
     while(!Queue.empty())  
     {
      u=Queue.front();         //Dequeue the gray painted Vertex at the head of the queue 
      Queue.pop();
      for(i=1;i<=TotalVertices;++i)      //For all adjacent Vertices 
      {
       for(j=1;j<=TotalVertices;++j)
       {
        v=j;       
        if(ResidualGraph[u][v]!=0 && color[v]==WHITE)//Is the Adjacent Vertex Undiscovered  
        {
         Queue.push(v); 
         color[v]=GRAY;        //The vertices in queue are always Gray painted
         Distance[v]=Distance[u]+1;
         parent[v]=u;
         if(v==sink)        //When the target is found ,just stop Dfs
          return true;      //Yup!!!! Sink still Reachable frm source on in Residual Network
        }
       }
       //Paint the Dequeued Vertex BLACK
       color[u]=BLACK;
      }
     }
     return false;
    }
    //FORD-FULKERSON
    int maxflow(int source,int sink)
    {
     int i,Rescap,x,y,max_flow=0;
     /*1.
     for each edge (u,v)E Graph 
     flow[u,v]:=0
     Already Done 
     */
     /*2. while there exists a path p from s to t in the residual network Gf*/
     while((bfs(source,sink)))        //True -> There exists an augmenting Path
     {
      Rescap=1<<30;          
      print_path(source,sink);       //Just Trace the Augmented Path
      for(i=0;i<path.size()-1;++i)
      {
       x=path[i],y=path[i+1];   
       if(ResidualGraph[x][y]<Rescap)     //Res Cap(p) :=min capacity in Residual network augmented path p.
        Rescap=ResidualGraph[x][y];
      }
      /*for each edge (x,y) in augmented path
      if x,y E Graph(originalNetwork).edge
      flow[x][y]+=Residual Capacity.
      else 
      flow[y][x]-=Residual Capacity.
      */
      for(i=0;i<path.size()-1;++i)
      {
       x=path[i],y=path[i+1];
       if(Graph[x][y]>0)
        flow[x][y]+=Rescap;
       else
        flow[y][x]-=Rescap;
    
       /*This one is important :
       We construct Residual Network by drawing the Back edge with flow value.
       ie --(f,C)->  .
       Then Residual Network is <------------f------------------
       ----OriginalCapaciy minus f --->
       */
       ResidualGraph[y][x]=flow[x][y];
       ResidualGraph[x][y]=Graph[x][y]-flow[x][y];  //Means I can send ResidualGraph[x][y] more through x,y pipe.
      }
      path.clear();
     }
     //Calculate Max Flow
     for(i=1;i<=TotalVertices;++i)
      max_flow=max_flow+flow[i][sink];
     return max_flow;
    }
    //Clear array for another Testcases
    void reset()
    {
     for(int i=0;i<TotalVertices+1;i++)
      for(int j=0;j<TotalVertices+1;j++)
       flow[i][j]=Graph[i][j]=ResidualGraph[i][j]=0;
    }
    int main()
    {
     int i,u,v,flowcap,source,sink,Kases=1;
     scanf("%d",&TotalVertices);
     for(;TotalVertices!=0;)
     {
      reset();
      scanf("%d%d%d",&source,&sink,&Totaledges);
      for(i=1;i<=Totaledges;++i)
      {
       scanf("%d %d %d",&u,&v,&flowcap);
       Graph[u][v]=Graph[u][v]+(flowcap);
       Graph[v][u]=Graph[v][u]+(flowcap);   //Uncomment this statement if the Graph is Bidirectional
    
       //Initially the Residual Graph
       ResidualGraph[u][v]=Graph[u][v];
       ResidualGraph[v][u]=Graph[u][v];   //Uncomment this statement if the Graph is Bidirectional
    
      }
      printf("Network %d\nThe bandwidth is %d.\n\n",Kases,maxflow(source,sink));
      Kases++;
      scanf("%d",&TotalVertices);
     }
     return 0;
    }
    
    
    

  
  
  


* * *

  
******_[Suffix Array](http://en.wikipedia.org/wiki/Suffix_array)_**  
**Suffix array is the most important data-structure. Suffix Array programming contest approach is best described in following[PDF](http://www.stanford.edu/class/cs97si/suffix-array.pdf) .**  
**Implementation 0: Following program computes Suffix Array. It also finds the longest Common Prefix length of adjacent Suffix. The program also implements RMQ to find the length of longest common prefix b/w any suffix i and j****.**  
**Tested at** : <https://www.hackerrank.com/challenges/string-similarity>  
  
****  
  

    
    
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */
    #include <stdio.h>
    #include <algorithm>
    #include <iostream>
    #include <string>
    #include <cstring>
    #include <cmath>
    #define MAX 100001
    
    
    using namespace std;
    char str[MAX]; //input
    int Rank[MAX], suffixArray[MAX]; //output
    int cnt[MAX], Next[MAX]; //internal
    bool bh[MAX], b2h[MAX];
    int Height[MAX];
    int Length_of_str;
    /*The format for M table where preprocessing value are stored is
    M[MAX_STRING_SIZE][logbase2(MAX_STRING_SIZE)].
    Also it it observed that Value of logbase2(10^7)= 23.253496664.
    Thus always fix logbase2 value to 25.
    */
    
    int M[MAX][25];
    
    bool smaller_first_char(int a, int b)
    {
     return str[a] < str[b];
    }
    void print(int index)
    {
     for(int i=index;i<strlen(str);++i)
     {
      cout<<str[i];
     }
     cout<<endl;
    }
    
    void suffixSort(int n)
    {
     //sort suffixes according to their first characters
     for (int i=0; i<n; ++i)
     {
      suffixArray[i] = i;
     }
     sort(suffixArray, suffixArray + n, smaller_first_char);
     //{suffixArray contains the list of suffixes sorted by their first character}
    
     for (int i=0; i<n; ++i)
     {
      bh[i] = i == 0 || str[suffixArray[i]] != str[suffixArray[i-1]];
      b2h[i] = false;
     }
    
     for (int h = 1; h < n; h <<= 1)
     {
      //{bh[i] == false if the first h characters of suffixArray[i-1] == the first h characters of suffixArray[i]}
      int buckets = 0;
      for (int i=0, j; i < n; i = j)
      {
       j = i + 1;
       while (j < n && !bh[j]) j++;
       Next[i] = j;
       buckets++;
      }
      if (buckets == n) break; // We are done! Lucky bastards!
      //{suffixes are separted in buckets containing strings starting with the same h characters}
    
      for (int i = 0; i < n; i = Next[i])
      {
       cnt[i] = 0;
       for (int j = i; j < Next[i]; ++j)
       {
        Rank[suffixArray[j]] = i;
       }
      }
    
      cnt[Rank[n - h]]++;
      b2h[Rank[n - h]] = true;
      for (int i = 0; i < n; i = Next[i])
      {
       for (int j = i; j < Next[i]; ++j)
       {
        int s = suffixArray[j] - h;
        if (s >= 0){
         int head = Rank[s];
         Rank[s] = head + cnt[head]++;
         b2h[Rank[s]] = true;
        }
       }
       for (int j = i; j < Next[i]; ++j)
       {
        int s = suffixArray[j] - h;
        if (s >= 0 && b2h[Rank[s]]){
         for (int k = Rank[s]+1; !bh[k] && b2h[k]; k++) b2h[k] = false;
        }
       }
      }
      for (int i=0; i<n; ++i)
      {
       suffixArray[Rank[i]] = i;
       bh[i] |= b2h[i];
      }
     }
     for (int i=0; i<n; ++i)
     {
      Rank[suffixArray[i]] = i;
     }
    }
    // End of suffix array algorithm
    
    /*
    Begin of the O(n) longest common prefix algorithm
    Refer to "Linear-Time Longest-Common-Prefix Computation in Suffix
    Arrays and Its Applications" by Toru Kasai, Gunho Lee, Hiroki
    Arimura, Setsuo Arikawa, and Kunsoo Park.
    */
    
    /*
    Note to say Suffix [i] always means the Ith suffix in LEXOGRAPHICALLY SORTED ORDER
    ie Height[i]=LCPs of (Suffix   i-1 ,suffix  i)
    */
    
    void getHeight(int n)
    {
     for (int i=0; i<n; ++i) Rank[suffixArray[i]] = i;
     Height[0] = 0;
     for (int i=0, h=0; i<n; ++i)
     {
      if (Rank[i] > 0)
      {
       int j = suffixArray[Rank[i]-1];
       while (i + h < n && j + h < n && str[i+h] == str[j+h])
       {
        h++;
       }
       Height[Rank[i]] = h;
       if (h > 0) h--;
      }
     }
    }
    // End of longest common prefixes algorithm
    
    /*When the LCP of consecutive pair of Suffixes is Knows 
    
    THEN:
    We can calculate the LCPs of any suffixes (i,j)
    with the Help of Following Formula
    
    ************************************************
    *  LCP(suffix i,suffix j)=LCP[RMQ(i + 1; j)]   * 
    *                                              *
    *  Also Note (i<j) As LCP (suff i,suff j) may  *
    *  not necessarly equal LCP (Suff j,suff i).   *
    ************************************************
    */
    
    void preprocesses(int N)
    {
     int i, j;
    
     //initialize M for the intervals with length 1
     for (i = 0; i < N; i++)
      M[i][0] = i;
    
     //compute values from smaller to bigger intervals
     for (j = 1; 1 << j <= N; j++)
     {
      for (i = 0; i + (1 << j) - 1 < N; i++)
      {
       if (Height[M[i][j - 1]] < Height[M[i + (1 << (j - 1))][j - 1]])
       {
        M[i][j] = M[i][j - 1];
       }
       else
       {
        M[i][j] = M[i + (1 << (j - 1))][j - 1];
       }
      }
     }
    }  
    int RMQ(int i,int j)
    {
     int k=log((double)(j-i+1))/log((double)2);
     int vv= j-(1<<k)+1 ;
     if(Height[M[i][k]]<=Height[ M[vv][ k] ])
      return M[i][k];
     else
      return M[ vv ][ k];
    }
    int LCP(int i,int j)
    {
     /*Make sure we send i<j always */
     /* By doing this ,it resolve following
     suppose ,we send LCP(5,4) then it converts it to LCP(4,5)
     */
     if(i>j)
      swap(i,j);
    
     /*conformation over*/
    
     if(i==j)
     {
      return (Length_of_str-suffixArray[i]);
     }
     else
     {
      return Height[RMQ(i+1,j)];
      //LCP(suffix i,suffix j)=LCPadj[RMQ(i + 1; j)] 
      //LCPadj=LCP of adjacent suffix =Height.
     }
    }
    int main()
    {
    
     int Len,TestCase,x,y;
     char tc[10];
     gets(tc);
     TestCase=atoi(tc);
    
     scanf("%d",&TestCase);
     while(TestCase--)
     {
      gets(str);
      Len=strlen(str);
      Length_of_str=Len;
    
      suffixSort(Len);
    
      /*Printing Suffix Array*/
      printf("The Suffix Array is: \n");
      for(int i=0;i<(Len);++i)
      {
       printf("%d %s\n",suffixArray[i],(str+suffixArray[i]));
      }
    
      /*Calculate LCP of Adjacents /consecutive Suffix Pairs */
      getHeight(Len);
    
      /*Calculate LCP of Any two suffixes i,j using RMQ concept */
      preprocesses(Len);
    
      int org=Rank[0];  //SA[org]=The suffix that is equal to the original String
    
      /*******************************/
      printf("\nPrinting Length of Longest Common Prefix Between Suffix i and j\n");
      for(int i=0;i<Len;++i)
      {
       for(int j=0;j<Len;++j)
       {
        printf("The Length of LCP b/w (%d,%d)th suffixes ie.(%s,%s) is: %d\n",i,j,(str+suffixArray[i]),(str+suffixArray[j]),LCP(i,j));
       }
      }
      /********************************/
    
      /*Calculation of LCP between orginal string and all its suffix*/
      printf("\n\nThe Sum of LCP b/w the original string S and all its suffixes is: ");
      int res=0;
      for(int i=0;i<Len;++i)
      {
       res=res+LCP(i,org);
      }
      cout<<res<<endl;
     }
     //system("pause");
     return 0;
    }
