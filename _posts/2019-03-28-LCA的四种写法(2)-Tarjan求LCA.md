#LCA专题之Tarjan

LCA(Lowest Common Ancestors)即最近公共祖先。在图论中，我们往往需要解决一些树上的操作，而很多时候我们需要求出某两个结点的最近公共祖先

Tarjan算法只能处理离线的求最近公共祖先问题，所谓离线，就是我们必须事先读取所有的询问，再跑Tarjan

Tarjan算法本身也是一次dfs,我们在dfs的过程中保存了如下信息，该节点是否访问vis[i]，该节点的父亲的编号fa[i]。
然后采用并查集思想缩点，如果该子树下所有的询问都已经处理完，就可以将该子树缩至根结点。
dfs过程如下
1、标记访问，记录父亲结点
2、对与该节点u有关的询问进行查询，如果询问的另一个点v已经被访问，则lca为get_f(v),find函数即并查集中的get_f(v),也就是缩点后的根节点。
3、对与该节点相连的所有子结点进行dfs



我们任然采用邻接表来存图，我们把询问也当作图，连边方便访问。
>
struct Edges{
	int s,t,nxt;
	Edges(){
		s = t = nxt = 0;
	}
	Edges(int _s,int _t,int _nxt):s(_s),t(_t),nxt(_nxt){}
}e[M*2],q[Q*2];
int tot = 1,head[N];
int get_f(int x){
	if(x == fa[x])return x;
	return fa[x] = get_f(fa[x]);
}
inline void add(int u,int v){
	e[++tot] = Edges(u,v,head[u]);
	head[u] = tot;
	e[++tot] = Edges(v,u,head[v]);
	head[v] = tot;
}
inline void add_q(int u,int v){
	q[++cont] = Edges(u,v,hq[u]);
	hq[u] = cont;
	q[++cont] = Edges(v,u,hq[v]);
	hq[v] =cont;
}
这里我用了pair<int,int>来保存每一对询问，然后映射这一对结点的lca上。
>
int Tarjan(int u){
	vis[u] = 1;
	fa[u] = u;
	int v;
	for(register int i=hq[u];i;i = q[i].nxt){
		v = q[i].t;
		if(vis[v]){
			lca[make_pair(v,u)] = lca[make_pair(u,v)] = get_f(v);	 
		}
	}
	for(int i = head[u];i;i = e[i].nxt){
		v = e[i].t;
		if(vis[v] == 0){
			dep[v] = dep[u]+1;
			Tarjan(v);
			fa[v] = u;
		}
	} 
}

这就是求LCA的Tarjan算法

>有空再配图