# autohair-annotation
1. 髪マスク作成
    ```
    cd face_parsing
    python get_hairmask.py
    ```
    - `get_hairmask.py` は `../images/` から画像を読み取り `../hair_masks/` に髪マスク画像，`../masked_hairs/` にマスクオーバーレイを保存する．
    - 出力画像サイズは(512,512)
    
2. 髪方向作成手法: 
    1. labelmeというソフトを使う
    
        [インストールと使い方はこちらを参考](https://github.com/wkentaro/labelme) 
            (pyqtを使うためmacosでは動かない可能性あり)

        1. `./masked_hairs` からマスクオーバーレイ画像を読み取る．
            <img width="400" src="for_readme/labelme_1.png"/>
        2. polygonで各髪領域を書いて，ラベルをつける (0,1,2,...整数を使ってください).
            <img width="400" src="for_readme/labelme_2.png"/>
        3. lineで各髪領域の方向を表す：
            - lineの原点が矢印の原点とみなして線を描く.
            - 対象髪領域と同じラベルをつける. **<== 大事**
            <img width="400" src="for_readme/labelme_3.png"/>
            <img width="400" src="for_readme/labelme_4.png"/>
            <img width="400" src="for_readme/labelme_5.png"/>
        4. jsonファイルを`./points_json/'へ保存する．
    2. 方向アノテーションを拡散してラベル付
        ```
        cd autohair
        python autohair.py
        ```
        - オリジナル画像を `../images/` から，<br />
        jsonファイルを　`../points_json/` から, <br />
        髪マスクを `../hair_masks/` から読み取る.
        - ラベルをつけて `../labeled_angles/` に出力
        - ラベル：1=[0, 0.5π), 2=[0.5π, π), 3=[π, 1.5π), 4=[1.5π, 2π), 5=髪領域と認識しているけど，labelmeで髪領域に指定してない, 0=髪領域ではない
