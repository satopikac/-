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
        queue<int> teamq;       //标记每个队的先后顺序
        queue<int> teamqueue[1005];     //为每一个队定义一个queue，存每次enqueue的元素
        string operations;
        cin>>operations;
        while(operations!="STOP")
        {
            if (operations=="ENQUEUE")
            {
               int NewNumber;
               cin>>NewNumber;
                if (teamqueue[team[NewNumber]].empty())    //如果新入队的元素没有属于它的队
                {
                    teamq.push(team[NewNumber]); //在teamq中添加新入队元素的队伍号码
                    teamqueue[team[NewNumber]].push(NewNumber);  //在teamqueue中加入新入队元素
                }
                else
                {
                    teamqueue[team[NewNumber]].push(NewNumber); //有自己的队，直接在teamqueue中加入新入队元素
                }
            }

            if (operations=="DEQUEUE")
            {
                cout<<teamqueue[teamq.front()].front()<<endl; //获取teamqueue首端元素
              teamqueue[teamq.front()].pop();    //从teamqueue首端弹出元素
              if(teamqueue[teamq.front()].empty())       //弹出的元素所在队没有元素了
              {
                  teamq.pop();             //在teamq中弹出这个队的号码
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

