# Logistic Regression

## 回顧
上一節課最後有提到，我們要找一個機率 posterior probability，當機率 P(X1|x) 大於 0.5 則輸出 C1 反之 C2。如果採用的是 Gaussian 機率分佈，我們可以說這個 posterior probability 就是 𝜎(𝑧)。而 z=w⋅x+b 當採用其它的機率分佈的話，最終得到的結果也是差不多的。 下標 w,b 代表這個 function set 是取決於 w,b 他們是透過訓練得到的一組參數，不同的 w,b 就會得到不同的 function。於是我們可以說 fw,b(x) 即為 posteriror probability。

![](https://i.imgur.com/u33dd6r.png)

## Step 1: Function Set
如果以圖像化表示會長這樣。我們的 function 有兩組參數，一組是 w 我們稱為 weight，另一個常數 b 稱為 bias。將 I 個輸入分別乘上 w 再加上 b 就可以得到 z，然後通過一個 sigmoid function 得到的輸出就是 posterior probability。以上就是一個 Logistic Regression 的運作機制。

![](https://i.imgur.com/Qg1RR78.png)

## 比較 Logistic Regression 與 Linear Regression 模型
這裏將 Logistic Regression 與 Linear Regression 兩者做一下比較。Logistic Regression 把每一個特徵都乘上一個 w 並加上 b。最後再通過 sigmoid function 然後輸出。因為輸出有經過一個 sigmoid function，因此輸出一定介於 0~1 之間。至於 Linear Regression 一樣把特徵乘上 w 再加上 b 就直接輸出，因此輸出可使是任何一個數值。兩著最大的差別就是線性迴歸模型的輸出並無經過一個 sigmoid function。

![](https://i.imgur.com/4cwIy4P.png)

## Step 2: Goodness of a Function
假設訓練資料集是依據我們所定義的 Posterior Probability 所產生的，因此只要給了一組的 w,b 就相對的決定了 Posterior Probability，就可以計算這組 w 與 b 產生該訓練資料集的機率。

![](https://i.imgur.com/JqbkZlR.png)

我們目標是要找一組 w 和 b 最大化 Likelihood，調整數學式將原本要找一組 w,b 最大化 L(w,b) 變更為 min−lnL(w,b) 。取 log 的好處就是相乘變相加，而加上負號就從最找最大變成找最小。我們將 C1,C2 以 0, 1 來表示就可以方便的計算。

![](https://i.imgur.com/utwsjnr.png)

我們原本是要找最大 Likelihood 的 function，換個角度從 L(w,b) 調整為 −ln⁡L(w,b)，並將類別 C1,C2 以 1,0 來表示。因此我們可以將最小化的目標寫成一個函數，即：

![](https://i.imgur.com/ES7oVGD.png)

而加總的部份其實是兩個 Bernoulli distribution 的 Cross entropy(交叉熵)。Cross entropy 代表 p 和 q 這兩個分佈有多接近，如果兩個分佈一樣的話，那所得即為0。 

![](https://i.imgur.com/uODlKjo.png)

我們可以發現這個 loss function 其實就是一個 binary cross entropy loss。

![](https://i.imgur.com/gUcCGdb.png)

## 比較 Logistic Regression 與 Linear Regression 損失函數
在 Logistic Regression 中我們定義的損失函數是要去最小化的對象是所有訓練資料 cross entropy 的總和。我們希望模型的輸出要跟目標答案要越接近越好。而 Linear Regression 相對單純只需要將真實答案與模型預測輸出做一個 Square Error 計算即可。

![](https://i.imgur.com/av1JisT.png)

## Step 3: Find the best function
第三步是尋找一組最好的參數，使得 loss 能夠最低。因此這裡採用梯度下降 (Gradient Descent) 來最小化交叉熵 (Cross Entropy)。作法上即是計算 −lnL(w,b) 對各 wi 的偏微分。那 lnL(w,b) 如何對 w 偏微分呢？我們知道 f 是受 z 這個變數影響。然而這個 z 是從 w, x, b 所產生的。因此我們可以將微分式子表示如下：

![](https://i.imgur.com/btwnYl9.png)

𝜕𝑧/𝜕𝑤𝑖 代表的是 xi，因為 z 只有一項與 wi*xi 有關。

![](https://i.imgur.com/xpV7e1R.png)

那前面這項的式子將 f(x) 換成 𝜎(z) 並作微分。𝜎(z) 是 sigmoid function，他的微分是 𝜎(z) * (1-𝜎(z))。

![](https://i.imgur.com/eZGcKbJ.png)

因此我們最後得到  (1-𝜎(z))*xi，其中 𝜎(z)  就是 f(x)。以上微分後的通式為 Cross Entropy 式子中的左項。

![](https://i.imgur.com/SaEDeuU.png)

Cross Entropy 式子中的右項也是一樣的推導方式。可以拆成 ln(1-f(x)) 先對 z 做偏微分，w 再對 z 做偏微分。而 𝜕𝑧/𝜕𝑤𝑖 代表的是 xi。最後得到 𝜎(z)*xi。

![](https://i.imgur.com/dmvPFch.png)

最後將上面推導的式子帶入之後，將變數移項並展開式子。並抵銷一樣的算式後，得到一個很直觀的結果。最終對 loss function 偏微分後的結果是蠻容易理解的。得到的結果就是每一項都是負的 Ground true(y) 減去模型所預測的值後並乘上 x 的每一項。

如果用 Gradient Descent 更新它的話就是當前的權重 wi 減去 Learning Rate(η) 乘上所有的訓練集帶入在 loss 偏微分的式子。權重的更新取決於三件事：

1. Learning Rate(η) 自己設置
2. xi 來自於訓練資料集中的每個輸入特徵
3. y^ 代表該筆資料的真實答案


![](https://i.imgur.com/JKAYvah.png)

## 比較 Logistic Regression 與 Linear Regression 參數更新方式
最後可以發現，Linear Regression 與 Logistic Regression 在更新的部份也是執行一樣的步驟，不同的部份在於 Logistic 的 y^ 是0或1，而 Linear 的 y^ 是任意實數。

![](https://i.imgur.com/MRPcDPa.png)

如果以 Linear Regression 的損失函數來求 Logistic Regression 的話，以 Class1 在 y^=1 的時候，不論 fw,b(x) 是0或1所得的偏微分都是0。這意味著套入公式後算出來離目標很近和目標很遠的時候算出來的微分都會等於0。

![](https://i.imgur.com/GYiaSv9.png)

如果我們將參數變化對 total loss 畫成圖的話。可以發現分類問題使用 Square  Error 的話在距離目標很遠和很近時梯度都是很平坦的。而使用適當的 Cross  Entropy ，距離目標越遠梯度就越大因此參數更新的幅度也會比較大，可以有個明確的目標幫助我們快速收斂到谷底。

![](https://i.imgur.com/1f95VPJ.png)

## Discriminative vs Generative
Logistic Regression 的方法稱作 Discriminative。而用高斯分佈來描述 posterior probability 這種方式稱作 Generative 方法。實際上兩者模型的 function set 是一樣的。只要做機率模型的時候把 covariance matrix 設定為共享的，其實兩者模型是一樣的 𝜎(𝑤 ∙ 𝑥 + 𝑏)。

如果是 Discriminative 方法就是透過梯度下降法將 w 與 b 找出來。如果是 Generative 方法的話我們會去算 𝜇1, 𝜇2, Σ−1 並將 w 與 b 算出來。

![](https://i.imgur.com/lUFvAWa.png)

Discriminative 與 Generative 雖然是同一個模型計算，但是最後的 function set 會是不一樣的結果。其因為是兩者做了不同的假設，所以跟去同一組訓練資料找出來的參數會是不一樣的。

> 在文獻上 Discriminative 會比 Generative 還來得好。

![](https://i.imgur.com/bgbbsIu.png)

## Generative vs. Discriminative
Generative 模型假設資料來自於一個機率模型，並做了某些假設。他在資料樣本少的狀況下，Generative 的優勢是會無視 data 而遵循它內心自己的假設，或是資料中帶有雜訊。在這些情況下有時候結果會是比 Discriminative 還來得好。

在做 Discriminative 的時候是直接假設一個 posterior probability，然後去找裡面的參數。而在做 Generative 的時候會將 function 拆成 Priors 與 class-dependent probabilities 這兩項。這件事情在語音辨識中是個重要關鍵。


![](https://i.imgur.com/1bq95JD.png)