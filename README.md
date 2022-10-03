假设有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给你一个整数数组  flowerbed 表示花坛，由若干 0 和 1 组成，其中 0 表示没种植花，1 表示种植了花。另有一个数 n ，能否在不打破种植规则的情况下种入 n 朵花？能则返回 true ，不能则返回 false。

 

示例 1：

输入：flowerbed = [1,0,0,0,1], n = 1
输出：true
示例 2：

输入：flowerbed = [1,0,0,0,1], n = 2
输出：false
 

提示：

1 <= flowerbed.length <= 2 * 104
flowerbed[i] 为 0 或 1
flowerbed 中不存在相邻的两朵花
0 <= n <= flowerbed.length
————————————————————————————————————————————————————————————————————————————————
解法一：跳格子
【1】当遍历到index遇到1时，说明这个位置有花，那必然从index+2的位置才有可能种花，因此当碰到1时直接跳过下一格。
【2】当遍历到index遇到0时，由于每次碰到1都是跳两格，因此前一格必定是0，此时只需要判断下一格是不是1即可得出index这一格能不能种花，如果能种则令n减一，然后这个位置就按照遇到1时处理，即跳两格；如果index的后一格是1，说明这个位置不能种花且之后两格也不可能种花（参照【1】），直接跳过3格。

当n减为0时，说明可以种入n朵花，则可以直接退出遍历返回true；如果遍历结束n没有减到0，说明最多种入的花的数量小于n，则返回false。

class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
    int i=0,len=flowerbed.size();
        while(i<len){
            if(flowerbed[i]==1){
                i+=2;
            }else if(i==len-1||flowerbed[i+1]==0){
                --n;
                i+=2;
            }else{
                i+=3;
            }
        }return n<=0;
    }

  ————————————————————————————————————————————————————————
   解法二：
  分别在开头和结尾多添加一个0
  class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
     if (flowerbed.size() == 0) return n == 0;
        int m=flowerbed.size();
        int countOfZero = 1; // 当前全0区段中连续0的数量，刚开始预设1个0，因为开头花坛的最左边没有花，可以认为存在一个虚无的0
        int canPlace = 0; // 可以种的花的数量
        for (int i=0;i<m;i++) {
            if (flowerbed[i] == 0) { // 遇到0，连续0的数量+1
                countOfZero++;
            } else { // 遇到1，结算上一段连续的0区间，看能种下几盆花：(countOfZero-1)/2
                canPlace += (countOfZero-1)/2;
                if (canPlace >= n) return true;
                countOfZero = 0; // 0的数量清零，开始统计下一个全0分区
            }
        }
        // 最后一段0区还未结算：
        countOfZero++; // 最后再预设1个0，因为最后花坛的最右边没有花，可以认为存在一个虚无的0
        canPlace += (countOfZero-1)/2;

        return canPlace >= n;
    }
};
贪心
贪心策略，从左到右遍历，能种花的条件是：

当前位置是0
且 前一位置是0或边界
且 后一位置是0或边界
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int size = flowerbed.size();
        for(int i=0; i<size; i++) {
            if (flowerbed[i]==0 && (i==0||flowerbed[i-1]==0) && (i==size-1||flowerbed[i+1]==0)) {
                n--;
                flowerbed[i] = 1;
            }
        }
        return n <= 0;
    }
};
