# Topic & Description

**Dataset**

1. stack_network_links.csv：
    
    
    | source | target | value |
    | --- | --- | --- |
    | asp.net | .net | 48.40702996199019 |
    | entity-framework | .net | 24.37090250532431 |
    | devops | amazon-web-services | 24.98353120101788 |
    | docker | amazon-web-services | 32.198071014100535 |
    | ios | android | 39.77803622570551 |
    | android-studio | android | 33.661083176336234 |
    | android | android-studio | 33.661083176336234 |
    
    描述技術標籤之間的連接，哪些技術標籤經常一起出現在開發者的履歷中，分析不同技術標籤之間的關係和關聯強度
    
    欄位：
    
    - source：與目標技術標籤相關的來源技術標籤
    - target：與來源技術標籤相關的目標技術標籤
    - value：表示這對技術標籤之間連接強度的數值。這個值可能表示這對技術標籤共同出現的頻率或相關度
2. stack_network_nodes.csv：
    
    
    | name | group | nodesize |
    | --- | --- | --- |
    | html | 6 | 272.45 |
    | css | 6 | 341.17 |
    | hibernate | 8 | 29.83 |
    | spring | 8 | 52.84 |
    
    網路中每個技術標籤的詳細訊息，分析技術標籤在整個網路中的分佈和重要性
    
    欄位：
    
    - name：技術標籤的名稱
    - group：技術標籤所屬的群組，表示相關技術標籤的分組這些群組是通過群集算法
    - nodesize：技術標籤的大小，表示該標籤被使用的頻率或流行度

**Purpose**

對於程式學習新手來說，要從哪一個程式語言開始起手、最適合新手的程式語言是什麼等問題是經典月經文，有些人學習完一個程式語言後往往不知道下一步該學習什麼技術。因此我們透過Stack overflow的技術標籤關聯網路分析相關技術標籤之間的關係，找出最為流行的數個程式語言或技術，以及與這些相關的技術，建立初學者的學習路徑。

1. 社群偵測：使用 `Leiden`、`Infomap` 演算法做社群檢測，找出哪些技術標籤經常一起出現形成一個社群，幫助理解可能需要一併學習的技術
2. 中心性分析：可以分析哪些技術標籤在網路中的位置最重要，也就是哪些技術標籤有最多的連接。這可以幫助我們找出最受歡迎或最重要的技術。ex: `katz centrality` , `in-degree centrality`, `load degree subgraph_degree_centrality`
3. 子網路分析：可以專注於網路的某一部分，例如只看 .NET 或 Android 相關的技術標籤，來深入理解這些技術的相關性。
4. 連接強度分析(鏈結預測分析方法)：可以分析哪些技術標籤之間的連接最強，也就是哪些技術標籤最常一起出現。分析推薦的順序
5. [視覺化資料內容](https://www.kaggle.com/code/chenxidong/stack-overflow-tag-networkx-visualization)：分析資料分布、使用igraph等方式繪製，各community之間關係

### Centrality Analysis

中心節點普遍被認為在社群網路中扮演重要角色，在本專題中我們以四種方法作為中心性衡量指標：程度中心性(Degree Centrality)、接近中心性(Clossness Centrality)、間接中間性(Betweenness Centrality)以及特徵向量中心性(Eigenvector Centrality)。

- 程度中心性：將「擁有愈多鄰居的節點」定義為中心節點，一個節點的程度中心性越高，表示它與越多的節點直接相連，可能在網路中扮演重要的角色

- 接近中心性：將「與其他節點之平均距離越短的節點」定義為中心，接近中心性越高的節點，表示它到其他節點的平均距離越短，代表它在網路中的位置更中心

- 間接中心性：將「頻繁被其他節點間之最短路徑經過的節點」定義為中心，間接中間性越高的節點，表示它在網路中的許多最短路徑上都出現，代表可能在網路中扮演**橋樑**的角色

- 特徵向量中心性：除了要有較高的程度中心性之外，還要與許多重要節點相連，因此該指標還計算此節點之鄰居對其重要程度的貢獻

在每種中心性指標中，我們分別找出前五名的中心節點與其鄰居，並將該子網路視覺化呈現於網路圖中，以利觀察何種技術與中心節點的相關度最高。


### 程度中心性(Degree Centrality)

在程度中心性部分，我們得出前五名最具程度中心性的節點為：`jquery`、`c#`、`css`、`asp.net`、`angularjs`，這些節點在網路中與其他節點的連結數最多，可能在網路中扮演重要角色。

| Index | Central Node | Value |
|-------|--------------|-------|
| 1     | jquery       | 0.2807 |
| 2     | c#           | 0.2456 |
| 3     | css          | 0.2456 |
| 4     | asp.net      | 0.2281 |
| 5     | angularjs    | 0.2281 |

![Degree Centrality](img/degree_centrality/bar_chart.png)

而各中心節點的子網路圖如下：

![Degree Centrality](img/degree_centrality/jquery_graph.png)
![Degree Centrality](img/degree_centrality/c_sharp_graph.png)
![Degree Centrality](img/degree_centrality/css_graph.png)
![Degree Centrality](img/degree_centrality/asp.net_graph.png)
![Degree Centrality](img/degree_centrality/angularjs_graph.png)


### 接近中心性(Closeness Centrality)

在接近中心性部分，我們得出前五名最具接近中心性的節點為：`jquery`、`mysql`、`ajax`、`css`、`javascript`，這些節點到其他節點的平均距離最短，可能在網路中的位置較中心。

| Index | Central Node | Value |
|-------|--------------|-------|
| 1     | jquery       | 0.2896 |
| 2     | mysql        | 0.2779 |
| 3     | ajax         | 0.2586 |
| 4     | css          | 0.2579 |
| 5     | javascript   | 0.2571 |

![Closeness Centrality](img/closeness_centrality/bar_chart.png)

而各中心節點的子網路圖如下：

![Closeness Centrality](img/closeness_centrality/jquery_graph.png)
![Closeness Centrality](img/closeness_centrality/mysql_graph.png)
![Closeness Centrality](img/closeness_centrality/ajax_graph.png)
![Closeness Centrality](img/closeness_centrality/css_graph.png)
![Closeness Centrality](img/closeness_centrality/javascript_graph.png)


### 間接中間性(Betweenness Centrality)

在間接中間性部分，我們得出前五名最具間接中間性的節點為：`jquery`、`linux`、`mysql`、`asp.net`、`apache`，這些節點在網路中的許多最短路徑上都出現，可能在網路中扮演橋樑的角色。

| Index | Central Node | Value |
|-------|--------------|-------|
| 1     | jquery       | 0.2555 |
| 2     | linux        | 0.2084 |
| 3     | mysql        | 0.1977 |
| 4     | asp.net      | 0.1741 |
| 5     | apache       | 0.1309 |

![Betweenness Centrality](img/betweenness_centrality/bar_chart.png)

而各中心節點的子網路圖如下：

![Betweenness Centrality](img/betweenness_centrality/jquery_graph.png)
![Betweenness Centrality](img/betweenness_centrality/linux_graph.png)
![Betweenness Centrality](img/betweenness_centrality/mysql_graph.png)
![Betweenness Centrality](img/betweenness_centrality/asp.net_graph.png)
![Betweenness Centrality](img/betweenness_centrality/apache_graph.png)


### 特徵向量中心性(Eigenvector Centrality)

在特徵向量中心性部分，我們得出前五名最具特徵向量中心性的節點為：`jquery`、`css`、`javascript`、`html5`、`php`，這些節點除了與其他節點的連結數最多之外，還與許多重要節點相連，可能在網路中扮演重要角色。

| Index | Central Node | Value |
|-------|--------------|-------|
| 1     | jquery       | 0.3658 |
| 2     | css          | 0.3387 |
| 3     | javascript   | 0.3256 |
| 4     | html5        | 0.2681 |
| 5     | php          | 0.2653 |

![Eigenvector Centrality](img/eigenvector_centrality/bar_chart.png)

而各中心節點的子網路圖如下：

![Eigenvector Centrality](img/eigenvector_centrality/jquery_graph.png)
![Eigenvector Centrality](img/eigenvector_centrality/css_graph.png)
![Eigenvector Centrality](img/eigenvector_centrality/javascript_graph.png)
![Eigenvector Centrality](img/eigenvector_centrality/html5_graph.png)
![Eigenvector Centrality](img/eigenvector_centrality/php_graph.png)

### 綜合比較

| Index | Degree Centrality | Closeness Centrality | Betweenness Centrality | Eigenvector Centrality |
|-------|-------------------|----------------------|------------------------|------------------------|
| 1     | jquery            | jquery               | jquery                 | jquery                 |
| 2     | c#                | mysql                | linux                  | css                    |
| 3     | css               | ajax                 | mysql                  | javascript             |
| 4     | asp.net           | css                  | asp.net                | html5                  |
| 5     | angularjs         | javascript           | apache                 | php                    |

可以看出這四種分析法的最大中心節點都是jquery，但是其他的中心節點就有些不同，這些分析法可以讓我們從不同的角度看待中心節點的重要性，提供更客觀的數據。