**倍增求lca**
```cpp
int lca(int u,int v){
    if(dep[u]<dep[v]) swap(u,v);
    for(int i=20;i>=0;i--) if(dep[f[u][i]]>=dep[v]) u=f[u][i];
    if(u==v) return u;
    for(int i=20;i>=0;i--) if(f[u][i]!=f[v][i]) u=f[u][i],v=f[v][i];
    return f[u][0];
}

```

**tarjan求lca**

通过并查集的思想，来确定 lca 。

每次进入一棵子树，把 f 值赋为自己，出去的时候 f 值赋为自己的父亲。

如果搜到一个属于一次询问的点 $u$，且另一个点 $v$ 已经被搜过，那么他们的 lca 就是 $v$ 通过并查集 f 所能回溯到的最高点（细品），即 find(v)。

```cpp
void dfs(int u,int fa){
    dep[u]=dep[fa]+1,f[u]=u;
    for(int i=head[u];i!=-1;i=g[i].nxt){
        int v=g[i].to;if(v==fa) continue;
        dfs(v,u);f[v]=u;
    }
    for(int i=head_quest[u];i!=-1;i=g[i].nxt){
        int it=g[i].to;int v=(q[it].l==u?q[it].r:q[it].l);
        if(f[v]&&!q[it].tag){
            int lca=find(v);
        }
    }
}

```

**树剖求lca**

最普通的树剖（脱去了所有的外壳$O(n\log n)$）

```cpp
int siz[N],top[N],dep[N],son[N],pa[N];
void dfs(int u,int fa){
    siz[u]=1,dep[u]=dep[fa]+1,pa[u]=fa;
    for(re int i=head[u];i!=-1;i=g[i].nxt){
        int v=g[i].to;if(v==fa) continue;
        dfs(v,u);siz[u]+=siz[v];
        if(siz[v]>siz[son[u]]) son[u]=v;
    }
}
void dfs2(int u,int fa,int topf){
    top[u]=topf;
    if(son[u]) dfs2(son[u],u,topf);
    for(re int i=head[u];i!=-1;i=g[i].nxt){
        int v=g[i].to;if(v==fa||v==son[u]) continue;
        dfs2(v,u,v);
    }
    return ;
}
int lca(int x,int y){
    while(top[x]!=top[y]){
        if(dep[top[x]]<dep[top[y]]) swap(x,y);
        x=pa[top[x]];
    }
    if(dep[x]>dep[y]) swap(x,y);
    return x;
}
```

**dfn序求lca**

咕咕咕。（了解，但是基本没敲过，不常用）