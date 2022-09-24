# NetheriteJam.github.io
Welcome to my github page.  
There are some interesting things in it.  
  
欢迎来到我的github页面  
这里有些好康的



N高度差很小，可以对这个宽度只有8的高度差状态压缩进行DP。每本书抽出来后，要么放在和它等高的书旁边，要不放在一边作为一个新的高度。
状态为：f[i][j][k][mask]，表示前i本书已经抽出了j本，前i本中没被抽出的书里最后一本书的高度是k，mask是一个0~2^8-1的二进
#include<bits/stdc++.h>
using namespace std;
int a[110],f[101][101][9][1<<8];
int main()
{
//  freopen("help.in","r",stdin);
//  freopen("help.out","w",stdout);
    memset(f,0x3f,sizeof(f));
    int n,k,ss=0;
    scanf("%d%d",&n,&k);
    for(int i=1;i<=n;i++)
    {
        scanf("%d",a+i);
        a[i]-=25;
        ss|=1<<a[i];
    }
    f[0][0][8][0]=0;
    //f[i][j][k][mask]，表示前i本书已经抽出了j本，
    //前i本中没被抽出的书里最后一本书的高度是k，mask是一个0~2^8-1的二进制，
    //表示前i本中没被抽出的书里高度的存在情况。
    //整体表示前i本书中没被抽出的书组成的序列的最小混乱度。
    for(int i=0;i<n;i++) //前i本书
        for(int j=0;j<=k;j++) //抽走了j本
            for(int l=0;l<=8;l++) //没抽走了，最后一本的高度
                for(int s=0;s<1<<8;s++) //没抽走了，存在哪些高度
                {
                    //第i个没有被抽走
                    f[i+1][j][a[i+1]][s|1<<a[i+1]]=min(f[i+1][j][a[i+1]][s|1<<a[i+1]],f[i][j][l][s]+(l!=a[i+1]));
                    if(j!=k) //如果还没有抽走K本，则第j本书是可以抽走的
                       f[i+1][j+1][l][s]=min(f[i+1][j+1][l][s],f[i][j][l][s]);
                }
    int ans=1e9;
    for(int s=0;s<1<<8;s++) //枚举高度集合，没有抽走的
    {
        int t=s^ss,cnt=0;
        //ss代表全集，t代表抽走的书的状态，如果本在没有抽走中的，为0,反之为1
        for(int i=0;i<8;i++) //计算T集合的大小
            cnt+=t>>i&1;
        for(int l=0;l<=8;l++)
            ans=min(f[n][k][l][s]+cnt,ans);
    }
    printf("%d\n",ans);
    return 0;
}
