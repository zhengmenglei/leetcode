# 一、题目
小明的电动车电量充满时可行驶距离为 cnt，每行驶 1 单位距离消耗 1 单位电量，且花费 1 单位时间。小明想选择电动车作为代步工具。地图上共有 N 个景点，景点编号为 0 ~ N-1。他将地图信息以 [城市 A 编号,城市 B 编号,两城市间距离] 格式整理在在二维数组 paths，表示城市 A、B 间存在双向通路。初始状态，电动车电量为 0。每个城市都设有充电桩，charge[i] 表示第 i 个城市每充 1 单位电量需要花费的单位时间。请返回小明最少需要花费多少单位时间从起点城市 start 抵达终点城市 end。      
      
**示例 1：**    
```
输入：paths = [[1,3,3],[3,2,1],[2,1,3],[0,1,4],[3,0,5]], cnt = 6, start = 1, end = 0, charge = [2,10,4,1]

输出：43

解释：最佳路线为：1->3->0。
在城市 1 仅充 3 单位电至城市 3，然后在城市 3 充 5 单位电，行驶至城市 5。
充电用时共 3*10 + 5*1= 35
行驶用时 3 + 5 = 8，此时总用时最短 43。
```
**示例 2：**      
```
输入：paths = [[0,4,2],[4,3,5],[3,0,5],[0,1,5],[3,2,4],[1,2,8]], cnt = 8, start = 0, end = 2, charge = [4,1,1,3,2]

输出：38

解释：最佳路线为：0->4->3->2。
城市 0 充电 2 单位，行驶至城市 4 充电 8 单位，行驶至城市 3 充电 1 单位，最终行驶至城市 2。
充电用时 4*2+2*8+3*1 = 27
行驶用时 2+5+4 = 11，总用时最短 38。
```
**提示：**    
- 1 <= paths.length <= 200
- paths[i].length == 3
- 2 <= charge.length == n <= 100
- 0 <= path[i][0],path[i][1],start,end < n
- 1 <= cnt <= 100
- 1 <= path[i][2] <= cnt
- 1 <= charge[i] <= 100
- 题目保证所有城市相互可以到达
       
       
来源：力扣（LeetCode）       
链接：[https://leetcode-cn.com/problems/DFPeFJ](https://leetcode-cn.com/problems/DFPeFJ)       
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。       
# 二、分析及代码    
## 1. 动态规划 + DFS
### （1）思路
设计一个动态规划数组 time[i][j]， 记录到达第 i 个城市剩余 j 电量的最小耗时。       
从起点开始，递归遍历可到达的城市，并更新 time 数组，直至下一可到达的目标城市所有充电值的成本均高于当前该城市对应成本，则停止当前支线的遍历。      
### （2）代码
```java
class Solution {
    int[][] len;//城市间距离
    int[] charge;//城市充电价格
    int n;//城市数量
    int cnt;//电动车满电电量
    int[][] time;//到第i个城市剩余j电量耗时
    
    public int electricCarPlan(int[][] paths, int cnt, int start, int end, int[] charge) {
        if (start == end)
            return 0;      
        this.charge = charge;
        this.n = charge.length;
        this.cnt = cnt;
        
        //连接矩阵，记录城市间距离
        this.len = new int[n][n];
        for (int i = 0; i < n; i++)//初始化
            for (int j = 0; j < n; j++)
                len[i][j] = 0;
        for (int[] path : paths) {
            int c1 = path[0], c2 = path[1], l = path[2];
            if (len[c1][c2] == 0 || len[c1][c2] > l) {//测试数据中两城市间可能给出多个距离，取最小值
                len[c1][c2] = l;
                len[c2][c1] = l;
            }
        }

        //动态规划数组初始化
        time = new int[n][cnt + 1];
        for (int i = 0; i < n; i++)
            for (int j = 0; j <= cnt; j++)
                time[i][j] = 100000000;//不可能达到的最大值
        for (int i = 0; i <= cnt; i++)
            time[start][i] = charge[start] * i;//在起点城市充电
        
        dfs(start);
        return time[end][0];      
    }
    
    public void dfs(int city) {
        //当前城市充电
        for (int i = 1; i <= cnt; i++)//充电后电量
            for (int left = 0; left < i; left++)//之前剩余电量
                time[city][i] = Math.min(time[city][i], time[city][left] + charge[city] * (i - left));
        
        //前往其他城市
        List<Integer> toVis = new ArrayList<Integer>();//下一步需更新的城市
        for (int i = 0; i < n; i++) {
            if (len[city][i] > 0) {
                int dis = len[city][i];
                boolean needVis = false;//如果前往目标城市后，所有充电值的成本均高于当前该城市对应成本，则无需前往
                for (int j = 0; j + dis <= cnt; j++) {
                    if (time[i][j] > time[city][j + dis] + dis) {
                        time[i][j] = time[city][j + dis] + dis;//更新该城市对应电量成本
                        needVis = true;
                    }
                }
                if (needVis == true)
                    toVis.add(i);//记录下一步需更新的城市
            }
        }

        //递归更新记录了的城市
        for (int i = 0; i < toVis.size(); i++)
            dfs(toVis.get(i));
        return;
    }
}
```
### （3）结果
执行用时 ：109 ms，在所有 Java 提交中击败了 45.76% 的用户；    
内存消耗 ：38.1 MB，在所有 Java 提交中击败了 100.00% 的用户。      
# 三、其他
暂无。  
