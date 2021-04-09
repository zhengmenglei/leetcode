# 一、题目
某解密游戏中，有一个 N*M 的迷宫，迷宫地形会随时间变化而改变，迷宫出口一直位于 (n-1,m-1) 位置。迷宫变化规律记录于 maze 中，maze[i] 表示 i 时刻迷宫的地形状态，"." 表示可通行空地，"#" 表示陷阱。      
地形图初始状态记作 maze[0]，此时小力位于起点 (0,0)。此后每一时刻可选择往上、下、左、右其一方向走一步，或者停留在原地。       
       
小力背包有以下两个魔法卷轴（卷轴使用一次后消失）：      
- 临时消除术：将指定位置在下一个时刻变为空地；
- 永久消除术：将指定位置永久变为空地。
       
       
请判断在迷宫变化结束前（含最后时刻），小力能否在不经过任意陷阱的情况下到达迷宫出口呢？      
注意： 输入数据保证起点和终点在所有时刻均为空地。       
      
**示例 1：**     
```
输入：maze = [[".#.","#.."],["...",".#."],[".##",".#."],["..#",".#."]]
输出：true
```
**示例 2：**    
```
输入：maze = [[".#.","..."],["...","..."]]
输出：false
解释：由于时间不够，小力无法到达终点逃出迷宫。
```
**示例 3：**      
```
输入：maze = [["...","...","..."],[".##","###","##."],[".##","###","##."],[".##","###","##."],[".##","###","##."],[".##","###","##."],[".##","###","##."]]
输出：false
解释：由于道路不通，小力无法到达终点逃出迷宫。
```
**提示：**     
- 1 <= maze.length <= 100
- 1 <= maze[i].length, maze[i][j].length <= 50
- maze[i][j] 仅包含 "."、"#"
       
来源：力扣（LeetCode）        
链接：[https://leetcode-cn.com/problems/Db3wC1](https://leetcode-cn.com/problems/Db3wC1)        
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。       
# 二、分析及代码    
## 1. 动态规划
### （1）思路
设计一个动态规划数组，记录各时间点、消除术使用状态下每个坐标的可达性。其中，两种消除术可表示为：
- 临时消除术，对当前状态为陷阱的位置，使用后当前状态变为可到达；
- 永久消除术，使用后该位置在所有时间点均可到达。
       
### （2）代码
```java
class Solution {
    int time, n, m;

    public boolean escapeMaze(List<List<String>> maze) {
        time = maze.size();
        n = maze.get(0).size();
        m = maze.get(0).get(0).length();
        if (time < n + m - 3)//时间过短无法到达
            return false;

        boolean[][][][][] visit = new boolean[time][n][m][2][2];//动态规划
        for (int t = 0; t < time; t++)//时间
            for (int i = 0; i < n; i++)//坐标x
                for (int j = 0; j < m; j++)//坐标y
                    for (int s1 = 0; s1 < 2; s1++)//消除术1状态
                        for (int s2 = 0; s2 < 2; s2++)//消除术2状态
                            visit[t][i][j][s1][s2] = false;
        visit[0][0][0][0][0] = true;//起点可到达
        int[][] points = new int[][]{{0,0},{0,-1},{0,1},{-1,0},{1,0}};

        for (int t = 1; t < time; t++) {
            for (int i = 0; i < n; i++) 
                for (int j = 0; j < m; j++)
                    for (int p = 0; p < 5; p++) {
                        int x = i + points[p][0];
                        int y = j + points[p][1];
                        if (x < 0 || x >= n || y < 0 || y >= m)
                            continue;
                        if (maze.get(t).get(i).charAt(j) == '.') {//当前位置可通行
                            for (int s1 = 0; s1 < 2; s1++)
                                for (int s2 = 0; s2 < 2; s2++)
                                    visit[t][i][j][s1][s2] |= visit[t - 1][x][y][s1][s2];
                        } else {
                            //使用临时消除术，该位置本次可停留
                            visit[t][i][j][1][0] |= visit[t - 1][x][y][0][0];
                            visit[t][i][j][1][1] |= visit[t - 1][x][y][0][1];

                            //使用永久消除术，该位置永久可停留
                            if (visit[t - 1][x][y][0][0] == true)
                                for (int t2 = t; t2 < time; t2++) 
                                    visit[t2][i][j][0][1] = true;
                            if (visit[t - 1][x][y][1][0] == true)
                                for (int t2 = t; t2 < time; t2++) 
                                    visit[t2][i][j][1][1] = true;
                        }
                    }
            for (int s1 = 0; s1 < 2; s1++)
                for (int s2 = 0; s2 < 2; s2++)
                    if (visit[t][n - 1][m - 1][s1][s2] == true)
                        return true;
        }
        return false;
    }
}
```
### （3）结果
执行用时 ：637 ms，在所有 Java 提交中击败了 21.38% 的用户；    
内存消耗 ：66.7 MB，在所有 Java 提交中击败了 46.89% 的用户。      
# 三、其他
需注意的时间点：小力从 t = 1 时刻才开始移动，因此 maze[0] （即地图的初始状态）实际上是无意义的。     
