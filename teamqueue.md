# teamqueue

```c++
#include <iostream>
#include <list>
#include <string>
using namespace std;
int main()
{

  int test=0; //the number of tests that has been operated
  int t;//the number of teams
  cin>>t;
  // cout<<"t is"<<t<<endl;
  while (t!=0)
  {
      test++;
      cout<<"Scenario #"<<test<<endl;
      int mark[200005]; //classify a number into its teams by giving array elements mark number
      int storenumber[200005];
      int add=0;
      for (int i = 0; i < t; ++i) {
                 int NumberInATeam; //the number of numbers a team possesses
                 cin>>NumberInATeam;
     //            cout<<"NumberInATeam is"<<NumberInATeam<<" ";
          for (int j = 0; j <NumberInATeam; ++j) {
              int number;
              cin>>number;
         //     cout<<"number is"<<number<<endl;
              storenumber[add]=number;
              mark[number]=i;
              add++;
          }
      }
      list <int> teamqueue;
      string operations;
      cin>>operations;
   //   cout<<operations;
      while (operations!="STOP")
      {

          if (operations=="DEQUEUE")
          {
              cout<<teamqueue.front()<<endl;
              teamqueue.pop_front();
         //     cout<<"popyes";
          }
          if (operations=="ENQUEUE")
          {
              int NumberWillIn;//numbers that waits to be pushed in the team queue
              cin>>NumberWillIn;
              int identification;
              for (int i = 0; i < add; ++i) {
                  if(storenumber[i]==NumberWillIn)
                  {
                      identification=mark[storenumber[i]];
                      break;
                  }
              }
        //      cout<<"identi is"<<identification<<endl;
              int flag=0;
              if(teamqueue.empty())
              {
                  teamqueue.push_back(NumberWillIn);
              }
              else {
                  for (auto it =(-- end(teamqueue)); it != (--begin(teamqueue)); it--) {
                    //  cout<<"*it "<<*it<<endl;
                      if (mark[*it] == identification) {
                       it++;
                          teamqueue.insert(it, NumberWillIn);
                          flag = 1;
                          break;
                      }

                  }

              if(flag==0) teamqueue.emplace_back(NumberWillIn);
              }
          }

          cin>>operations;
      }

      if (operations=="STOP") cout<<endl;


      cin>>t;
  }

  return 0;

}
```

TIME LIMIT EXCEEDED

A POSSIBLE VERSION

```c++
#include <iostream>
#include <queue>
#include <string>
#include <map>

using namespace std;

int main()
{
    int test=0; //the number of tests that has been operated
    int t;//the number of teams
    cin>>t;
    while(t!=0)
    {
        test++;
        cout<<"Scenario #"<<test<<endl;
        map<int,int> team; //key 具体某个数字 value 它在哪个队

        for (int i = 0; i < t; ++i) {
            int NumberInATeam; //the number of numbers a team possesses
            cin>>NumberInATeam;
            for (int j = 0; j < NumberInATeam; ++j) {
                int number;
                cin>>number;
                team[number]=i;  //number被分配给第i队
            }
        }
        queue<int> teamq;
        queue<int> teamqueue[1005];
        string operations;
        cin>>operations;
        while(operations!="STOP")
        {
            if (operations=="ENQUEUE")
            {
               int NewNumber;
               cin>>NewNumber;
                if (teamqueue[team[NewNumber]].empty())
                {
                    teamq.push(team[NewNumber]);
                    teamqueue[team[NewNumber]].push(NewNumber);
                }
                else
                {
                    teamqueue[team[NewNumber]].push(NewNumber);
                }
            }

            if (operations=="DEQUEUE")
            {
                cout<<teamqueue[teamq.front()].front()<<endl;
              teamqueue[teamq.front()].pop();
              if(teamqueue[teamq.front()].empty())
              {
                  teamq.pop();
              }
            }
            
            cin>>operations;
        }
        if (operations=="STOP") cout<<endl;


        cin>>t;
    }

    return 0;
}
```

