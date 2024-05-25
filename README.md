# Matlab 籃球表現數據分析：主成分分析及群集分析
## 主旨
利用主成分分析及群集分析來分類不同的進攻策略，並分析其結果

## 分析過程

### 主成分分析
將一些無關的資料 column 去除後 (如隊伍名稱等)，進行主成分分析 (PCA)
```matlab=
[coeff, score, latent, tsquared, explained] = pca(table2array(data));
```
進而畫出解說變異量百分圖和主成分得分圖

#### 解說變異量百分圖
![](https://hackmd.io/_uploads/ryJ8xrJV0.png)
這張圖顯示每個資料群集的重要性，可以看到，前四個主成分大約解釋了 70% 的變異量，因此我們將取這四個主成分去做進一步的分析

#### 主成分得分圖
![](https://hackmd.io/_uploads/rkMHdr1NC.png)
##### 圖表解釋
* 橫軸及縱軸為第一和第二主成分
* 紅色點代表一個觀測直在前兩個主成分上的得分
* 藍色箭頭為一個變數對主成分的貢獻

我們可以以此表觀察每個變量間的相關程度，這個之後會使用到

### K-means 群集分析
我們取前四個主成分做群集分析
```matlab=
numClusters = 4;
[idx, C] = kmeans(score(:,1:numClusters), numClusters);
clusteredData = addvars(data, idx, 'NewVariableNames', 'Cluster');
```
![](https://hackmd.io/_uploads/H1VERrkNC.png)
![](https://hackmd.io/_uploads/H1kSRByVA.png)
![](https://hackmd.io/_uploads/HkKH0SyNC.png)


根據此圖表我們可以簡單分析：
1. **每百回合進攻效率** 越高，**獲勝的場數** 會愈高
2. **對手有效投籃命中率** 越高，**每百回合防守效率** 愈高
