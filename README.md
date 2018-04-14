# Tram

Time Limit: 1000MS		Memory Limit: 30000K

Total Submissions: 17673		Accepted: 6599

**Description**

Tram network in Zagreb consists of a number of intersections and rails connecting some of them. In every intersection there is a switch pointing to the one of the rails going out of the intersection. When the tram enters the intersection it can leave only in the direction the switch is pointing. If the driver wants to go some other way, he/she has to manually change the switch. 

When a driver has do drive from intersection A to the intersection B he/she tries to choose the route that will minimize the number of times he/she will have to change the switches manually. 

Write a program that will calculate the minimal number of switch changes necessary to travel from intersection A to intersection B. 

**Input**

The first line of the input contains integers N, A and B, separated by a single blank character, 2 <= N <= 100, 1 <= A, B <= N, N is the number of intersections in the network, and intersections are numbered from 1 to N. 

Each of the following N lines contain a sequence of integers separated by a single blank character. First number in the i-th line, Ki (0 <= Ki <= N-1), represents the number of rails going out of the i-th intersection. Next Ki numbers represents the intersections directly connected to the i-th intersection.Switch in the i-th intersection is initially pointing in the direction of the first intersection listed. 

**Output**

The first and only line of the output should contain the target minimal number. If there is no route from A to B the line should contain the integer "-1".

**Sample Input**

3 2 1
2 2 3
2 3 1
2 1 2

**Sample Output**

0

## 方法：DIJ+HEAP模板


## 代码：

C++:


    #include <iostream>
    #include <cstdio>
    #include <queue>

	using namespace std;

	struct qnode {
		int v, c;//c--cost  
		qnode(int vv = 0, int cc = 0) :v(vv), c(cc) {}
		bool operator < (const qnode& r) const { return c>r.c; }
	};
	priority_queue<qnode> q;
	const int inf = 0x3fffffff;
	int v[200][200];
	int vis[200];
	int dis[200];

	int dij(int n, int a, int b)
	{
		int i, j;
		for (i = 1; i <= n; i++)//初始化节点
		{
			dis[i] = v[a][i];//也可初始化为inf，不过下面的vis[s]=1必须去掉
			if (dis[i] != inf)
			{
				qnode x(i, dis[i]);
				q.push(x);
			}
				
			vis[i] = 0;
		}
		dis[a] = 0;

		while (!q.empty())
		{
			int now = q.top().v;
			q.pop();
			vis[now] = 1;
			for (j = 1; j <= n; j++)//松弛
				if (vis[j] == 0 && dis[j] > dis[now] + v[now][j])//标记过的点都是比当前点近的点，通过当前点不能松弛比它更接近源点的点
				{
					dis[j] = dis[now] + v[now][j];
					qnode x(j, dis[j]);
					q.push(x);
				}
		}
		
		return (dis[b] == inf) ? -1 : dis[b];

	}

	int main()
	{
		int n, a, b;
		while (~scanf("%d %d %d", &n, &a, &b))
		{
			for (int i = 1; i <= n; i++)
			{
				dis[i] = inf;
				vis[i] = 0;
				for (int j = 1; j <= n; j++)
				{
					v[i][j] = inf;
				}
			}


			for (int i = 1; i <= n; i++)
			{
				int num;
				scanf("%d", &num);

				for (int j = 0; j < num; j++)
				{
					int temp;
					scanf("%d", &temp);
					if (!j)
						v[i][temp] = 0;
					else
						v[i][temp] = 1;
				}
			}

			printf("%d\n", dij(n, a, b));
		}

		return 0;
	}
