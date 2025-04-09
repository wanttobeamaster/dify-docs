# メタデータ

## メタデータとは？

### 定義

メタデータは他のデータを説明するための情報です。簡単に言えば、「データに関するデータ」です。これは本の目次やラベルのようなもので、データの内容、出所、用途を紹介します。
データの文脈を提供することで、メタデータはナレッジベース内でのデータの迅速な検索と管理を支援します。

### ナレッジベースのメタデータ定義

-   **フィールド（Field）**：メタデータフィールドは文書の特定の属性を説明するための識別項目で、各フィールドは文書のある特徴や情報を表します。例えば「author」「language」など。

-   **フィールド値（Value）**：フィールド値はそのフィールドの具体的な情報や属性です。例えば「Jack」「English」。

<p align="center">
  <img src="https://assets-docs.dify.ai/2025/03/b6a197aa21ab92db93869fcbfa156b62.png" width="300" alt="Field name and value">
</p>

-   **フィールド値カウント（Value Count）**：フィールド値カウントは、あるメタデータフィールドにマークされているフィールド値の数を指し、重複項目も含みます。例えば、ここでの「3」はフィールド値カウントであり、そのフィールドに3つのユニークなフィールド値があることを示します。

<p align="center">
  <img src="https://assets-docs.dify.ai/2025/03/330f26e90438cf50167c4cb6ce30e458.png" width="300" alt="Metadata field">
</p>

-   **値タイプ（Value Type）**：値タイプはフィールド値の種類を指します。
    -   現在、Difyのメタデータ機能は以下の三種類の値タイプをサポートしています：
        -   **文字列**（String）：テキスト値。
        -   **数値**（Number）：数値。
        -   **時間**（Time）：日付と時間。

<p align="center">
  <img src="https://assets-docs.dify.ai/2025/03/f6adc7418869334805361535c8cd6874.png" width="300" alt="Value type">
</p>

## ナレッジベースのメタデータはどのように管理するか？

### ナレッジベースのメタデータフィールドの管理

ナレッジベース管理インターフェースでは、メタデータフィールドの作成、修正、削除ができます。

> 注意：このインターフェースで行うすべての更新は**グローバル更新**であり、メタデータフィールドリストへの変更はナレッジベース全体に影響し、すべての文書にマークされたメタデータを含みます。

#### メタデータ管理インターフェースの紹介

**メタデータ管理インターフェースへのアクセス**

ナレッジベース管理インターフェースで、右上の **メタデータ** ボタンをクリックして、メタデータ管理インターフェースに入ります。

![Entrance of Metadata Panel](https://assets-docs.dify.ai/2025/03/bd43305d49cc1511683b4a098c8f6e5a.png)

![New metadata](https://assets-docs.dify.ai/2025/03/6000c85b5d2e29a2a5af5e0a047a7a59.png)

**ナレッジベースのメタデータフィールドの種類**

ナレッジベースでは、メタデータフィールドは**ビルトインメタデータ（Built-in）**と**カスタムメタデータ**の二種類に分かれます。

<table border="0" cellspacing="0" cellpadding="10" style="width: 100%; border-collapse: collapse;">
    <tr>
        <th style="width: 15%; text-align: center; background-color: #f5f5f5;"></th>
        <th style="width: 42.5%; text-align: center; background-color: #f5f5f5;">ビルトインメタデータ（Built-in）</th>
        <th style="width: 42.5%; text-align: center; background-color: #f5f5f5;">カスタムメタデータ</th>
    </tr>
    <tr>
        <td style="text-align: center;">表示位置</td>
        <td>ナレッジベースインターフェースのメタデータ欄の下半分。</td>
        <td>ナレッジベースインターフェースのメタデータ欄の上半分。</td>
    </tr>
    <tr>
        <td style="text-align: center;">有効化方法</td>
        <td>デフォルトでは無効で、手動で有効にする必要があります。</td>
        <td>ユーザーが必要に応じて自由に追加できます。</td>
    </tr>
    <tr>
        <td style="text-align: center;">生成方法</td>
        <td>有効化後、システムが自動的に関連情報を抽出してフィールド値を生成します。</td>
        <td>ユーザーが手動で追加し、完全にユーザー定義です。</td>
    </tr>
    <tr>
        <td style="text-align: center;">修正権限</td>
        <td>一度生成されると、フィールドとフィールド値は修正できません。</td>
        <td>フィールド名の削除や編集が可能で、フィールド値も修正できます。</td>
    </tr>
    <tr>
        <td style="text-align: center;">適用範囲</td>
        <td>有効化後、既にアップロードされた文書と新たにアップロードされるすべての文書に適用されます。</td>
        <td>メタデータフィールドを追加した後、フィールドはナレッジベースのメタデータリストに保存されます。具体的な文書にそのフィールドを適用するには手動設定が必要です。</td>
    </tr>
    <tr>
        <td style="text-align: center;">フィールド</td>
        <td>
            システムによって事前定義され、以下を含みます：<br>
            • document_name (string)：ファイル名<br>
            • uploader (string)：アップローダー<br>
            • upload_date (time)：アップロード日<br>
            • last_update_date (time)：最終更新時間<br>
            • source (string)：ファイルソース
        </td>
        <td>初期状態では、ナレッジベースにカスタムメタデータフィールドはなく、ユーザーが手動で追加する必要があります。</td>
    </tr>
    <tr>
        <td style="text-align: center;">フィールド値タイプ</td>
        <td>
            • 文字列 (string)：テキスト値<br>
            • 数値 (number)：数値<br>
            • 時間 (time)：日付と時間
        </td>
        <td>
            • 文字列 (string)：テキスト値<br>
            • 数値 (number)：数値<br>
            • 時間 (time)：日付と時間
        </td>
    </tr>
</table>

#### メタデータフィールドの新規作成

1.  **+メタデータを追加** ボタンをクリックすると、**メタデータの新規作成** ポップアップが表示されます。

![New metadata](https://assets-docs.dify.ai/2025/03/5086db42c40be64e54926b645c38c9a0.png)

2.  **フィールド値タイプ** でメタデータフィールドの値タイプを選択します。

3.  **名前** ボックスにフィールドの名前を入力します。

> フィールド名は小文字、数字、アンダースコア（_）のみをサポートし、スペースや大文字はサポートしていません。

<p align="center">
  <img src="https://assets-docs.dify.ai/2025/03/f6adc7418869334805361535c8cd6874.png" width="300" alt="Value type">
</p>

4.  **保存** ボタンをクリックして、フィールドを保存します。

![Save field](https://assets-docs.dify.ai/2025/03/f44114cc58d4ba11ba60adb2d04c9b4c.png)

#### メタデータフィールドの修正

1.  単一のメタデータフィールドの右側にある編集ボタンをクリックすると、**リネーム** ポップアップが表示されます。

![Rename field](https://assets-docs.dify.ai/2025/03/94327185cbe366bf99221abf2f5ef55a.png)

2.  **名前** ボックスでフィールド名を修正します。

> このポップアップではフィールド名の修正のみをサポートし、フィールド値タイプの修正はサポートしていません。

<p align="center">
  <img src="https://assets-docs.dify.ai/2025/03/2f814f725df9aeb1a0048e51d736d969.png" width="350" alt="Rename field">
</p>

3.  **保存** ボタンをクリックして、修正後のフィールドを保存します。

> 修正して保存すると、そのフィールドはナレッジベース内のすべての関連文書で同期更新されます。

![Renamed field](https://assets-docs.dify.ai/2025/03/022e42c170b40c35622b9b156c8cc159.png)

#### メタデータフィールドの削除

単一のメタデータフィールドの右側にある削除ボタンをクリックすると、そのフィールドを削除できます。

> 単一のフィールドを削除すると、そのフィールドおよびそのフィールドに含まれるフィールド値がナレッジベースのすべての文書から削除されます。

![Delete field](https://assets-docs.dify.ai/2025/03/022e42c170b40c35622b9b156c8cc159.png)

### 文書メタデータ情報の編集

#### 文書メタデータ情報の一括編集

ナレッジベース管理インターフェースで文書のメタデータ情報を一括編集できます。

**メタデータ編集ポップアップを開く**

1. ナレッジベース管理インターフェースを開き、文書リストの左側にある白いボックスで一括操作したい文書をチェックします。チェックすると、ページの下部に操作オプションが表示されます。

![Entrance of Edit Metadata](https://assets-docs.dify.ai/2025/03/18b0c435604db6173acba41662474446.png)

2. 操作オプションの **メタデータ** をクリックすると、**メタデータの編集** ポップアップが表示されます。

![Edit metadata](https://assets-docs.dify.ai/2025/03/719f3c31498f23747fed7d7349fd64ba.png)

**メタデータ情報の一括追加**

1.  **メタデータの編集** ポップアップの下部にある **+メタデータを追加** ボタンをクリックすると、操作ポップアップが表示されます。

<p align="center"><img src="https://assets-docs.dify.ai/2025/03/d4e4f87447c3e445d5b7507df1126c7b.png" width="400" alt="Add metadata"></p>

-  選択した文書に既に作成されたフィールドを追加する場合：

    -   ドロップダウンリストから既存のフィールドを選択して、フィールドリストに追加できます。
 
    -   **メタデータを検索** 検索ボックスで必要なフィールドを検索し、その文書のフィールドリストに追加できます。
    
    <p align="center"><img src="https://assets-docs.dify.ai/2025/03/ea9aab2c4071bf2ec75409b05725ac1f.png" width="400" alt="Existing field"></p>

-  選択した文書に新しいフィールドを作成する場合は、ポップアップの左下にある **+メタデータを新規作成** ボタンをクリックし、前述の **メタデータフィールドの新規作成** セクションを参照してフィールドを作成できます。
    
    > **+メタデータを新規作成** ポップアップで作成したメタデータフィールドは、自動的にナレッジベースのフィールドリストに同期されます。

    <p align="center"><img src="https://assets-docs.dify.ai/2025/03/e32211f56421f61b788943ba40c6959e.png" width="400" alt="New metadata field"></p>
    
-  既に作成されたフィールドを管理する場合は、そのポップアップの右下にある **管理** ボタンをクリックすると、ナレッジベースの管理インターフェースに移動します。

    <p align="center"><img src="https://assets-docs.dify.ai/2025/03/82561edeb747b100c5295483c6238ffa.png" width="400" alt="Manage field"></p>

2.  *（オプション）* フィールドを追加した後、フィールド値ボックスにそのフィールドに対応するフィールド値を入力します。

<p align="center"><img src="https://assets-docs.dify.ai/2025/03/aabfe789f607a1db9062beb493213376.png" width="400" alt="Value for field"></p>

-  値タイプが **時間** の場合、フィールド値を入力する際に時間選択ツールが表示され、具体的な時間を選択できます。

<p align="center"><img src="https://assets-docs.dify.ai/2025/03/65df828e605ebfb4947fccce189520a3.png" width="440" alt="Date picker"></p>

3.  **保存** ボタンをクリックして、操作を保存します。

**メタデータ情報の一括削除と修正**

1.  **メタデータの編集** ポップアップでメタデータ情報を削除・修正します：

-   **フィールド値の追加**： メタデータ値を追加したいフィールドボックス内に直接必要な値を入力します。

-   **フィールド値のリセット**： カーソルをフィールド名の左側にある青い点の上に置くと、青い点は **リセット** ボタンに変わります。青い点をクリックすると、フィールドボックス内の修正された内容が元のメタデータ値にリセットされます。

    <p align="center"><img src="https://assets-docs.dify.ai/2025/03/01c0cde5a6eafa48e1c6e5438fc2fa6b.png" width="400" alt="Reset values"></p>

-   **フィールド値の削除**：

    -   1つのフィールド値を削除する：削除したいフィールド値のフィールドボックス内で直接そのフィールド値を削除します。

    -   複数のフィールド値を削除する：**複数の値** カードの削除アイコンをクリックして、選択したすべての文書のそのメタデータフィールドの値を消去します。

        <p align="center"><img src="https://assets-docs.dify.ai/2025/03/5c4323095644d2658881b783246914f1.png" width="400" alt="Multiple values"></p>

-   **単一メタデータフィールドの削除**： フィールドの最右側にある削除記号をクリックして、そのフィールドを削除します。削除後、そのフィールドは横線で消され、グレーアウトします。
    
    > この操作は選択した文書のそのフィールドとフィールド値のみを削除し、フィールド自体はナレッジベースに保持されます。

    <p align="center"><img src="https://assets-docs.dify.ai/2025/03/1b0318b898f951e307e3dc8cdc2f48d3.png" width="400" alt="Delete fields"></p>

2.  **保存** ボタンをクリックして、操作を保存します。

**一括操作の適用範囲の調整**

- **一括操作の適用範囲の調整**： **メタデータの編集** ポップアップの左下にある **すべての文書に適用** チェックボックスを使用して、編集モードでの変更の適用範囲を調整できます。

    -   **いいえ（デフォルト）**： このオプションをチェックしない場合、編集モードでの変更は元々そのメタデータフィールドを持っていた文書にのみ適用され、他の文書には影響しません。

    -   **はい**： このオプションをチェックする場合、編集モードでの変更はすべての選択された文書に適用されます。元々そのフィールドを持っていなかった文書には、自動的にそのフィールドが追加されます。

<p align="center"><img src="https://assets-docs.dify.ai/2025/03/4550c68960802c24271492b63a39ad05.png" width="400" alt="Apply all changes"></p>

#### 単一文書のメタデータ情報の編集

文書詳細インターフェースで単一文書のメタデータ情報を編集できます。

**文書メタデータ編集モードへのアクセス**

1.  文書詳細インターフェースで、情報バーの上部にある **ラベリングを開始** ボタンをクリックします。

![Details page](https://assets-docs.dify.ai/2025/03/066cb8eaa89f6ec17aacd8b09f06771c.png)

2.  文書メタデータ編集モードに入ります。

![Start labeling](https://assets-docs.dify.ai/2025/03/4806c56e324589e1711c407f6a1443de.png)

**メタデータの文書追加**

1. 文書のメタデータ編集モードで、**+メタデータを追加**ボタンをクリックすると、操作ポップアップが表示されます。
![Add metadata](https://assets-docs.dify.ai/2025/03/f9ba9b10bbcf6eaca787eed4fcde44da.png)

- 新しいフィールドを作成してその文書にフィールド値をマークしたい場合は、ポップアップの左下にある**+ メタデータを新規作成**ボタンをクリックし、前述の**メタデータフィールドの新規作成**セクションを参照してフィールドを作成できます。

    > 文書ページで作成された新しいメタデータフィールドは、自動的にナレッジベースのフィールドリストに同期されます。
        
    ![New fields](https://assets-docs.dify.ai/2025/03/739e7e51436259fca45d16065509fabb.png)

- ナレッジベースに既存のフィールドを使用してその文書にフィールド値をマークしたい場合は、以下のいずれかの方法で既存のフィールドを使用できます：

    - ドロップダウンリストからナレッジベースの既存のフィールドを選択し、その文書のフィールドリストに追加します。

    - **メタデータを検索**検索ボックスで必要なフィールドを検索し、その文書のフィールドリストに追加します。
    
    ![Existing fields](https://assets-docs.dify.ai/2025/03/5b1876e8bc2c880b3b774c97eba371ab.png)

- ナレッジベースの既存のフィールドを管理したい場合は、ポップアップの右下にある**管理**ボタンをクリックして、ナレッジベースの管理インターフェースに移動します。

    ![Manage metadata](https://assets-docs.dify.ai/2025/03/8dc74a1d2cdd87294e58dbc3d6dd161b.png)

2. *（オプション）* フィールドを追加した後、フィールド名の右側のメタデータ欄にフィールド値を入力します。

![Values for fields](https://assets-docs.dify.ai/2025/03/488107cbea73fd4583e043234fe2fd2e.png)

3. 右上の**保存**ボタンをクリックして、フィールド値を保存します。

**文書メタデータ情報の削除と編集**

1. 文書のメタデータ編集モードで、右上の**編集**ボタンをクリックして、編集モードに入ります。

![Edit mode](https://assets-docs.dify.ai/2025/03/bb33a0f9c6980300c0f979f8dc0d274d.png)

2. 文書メタデータ情報を削除・編集します：
    - **フィールド値の削除と編集**：フィールド名の右側のフィールド値ボックス内で、フィールド値を削除または修正します。

    > このモードではフィールド値の修正のみをサポートし、フィールド名の修正はサポートしていません。

    - **フィールドの削除**：フィールド値ボックスの右側にある削除ボタンをクリックして、フィールドを削除します。

    > この操作はその文書のそのフィールドとフィールド値のみを削除し、フィールド自体はナレッジベースに保持されます。

![Edit metadata](https://assets-docs.dify.ai/2025/03/4c0c4d83d3ad240568f316abfccc9c2c.png)

3. 右上の**保存**ボタンをクリックして、修正後のフィールド情報を保存します。

## メタデータ機能を使用してナレッジベース内の文書をフィルタリングする方法は？

[アプリケーション内でナレッジベースを統合する](https://docs.dify.ai/ja-jp/guides/knowledge-base/integrate-knowledge-within-application)の**メタデータを使用して知識をフィルタリングする**セクションを参照してください。

## API情報

[APIを通じてナレッジベースを維持する](https://docs.dify.ai/ja-jp/guides/knowledge-base/knowledge-and-documents-maintenance/maintain-dataset-via-api)を参照してください。

## FAQ

- **メタデータの役割は何ですか？**

    - 検索効率の向上：ユーザーはメタデータタグを使用して関連情報を素早くフィルタリングして検索でき、時間を節約し作業効率を高めます。

    - データセキュリティの強化：メタデータを通じてアクセス権限を設定し、許可されたユーザーのみが機密情報にアクセスできるようにすることで、データのセキュリティを確保します。

    - データ管理能力の最適化：メタデータは企業や組織が効果的にデータを分類・保存するのを助け、データの管理と検索能力を向上させ、データの可用性と一貫性を強化します。

    - 自動化プロセスのサポート：メタデータは文書管理やデータ分析などのシナリオでタスクや操作を自動的にトリガーし、プロセスを簡素化して全体の効率を高めます。

- **ナレッジベースメタデータ管理リスト内のメタデータフィールドと特定の文書内のメタデータ値には、どのような違いがありますか？**

<table style="width: 100%; border-collapse: collapse; background-color: #f8f9ff;">
    <thead>
        <tr>
            <th style="width: 25%; padding: 12px; border: 1px solid #e5e7eb; background-color: #f9fafb;">/</th>
            <th style="width: 25%; padding: 12px; border: 1px solid #e5e7eb; background-color: #f9fafb;">定義</th>
            <th style="width: 25%; padding: 12px; border: 1px solid #e5e7eb; background-color: #f9fafb;">性質</th>
            <th style="width: 50%; padding: 12px; border: 1px solid #e5e7eb; background-color: #f9fafb;">例</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">メタデータ管理リスト内のメタデータフィールド</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">文書の特定の属性を説明するための事前定義されたフィールド。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">グローバルフィールド。すべての文書がこれらのフィールドを使用できます。</td>
           <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">著者、文書タイプ、アップロード日。</td>
        </tr>
        <tr>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">特定の文書内のメタデータ値</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">各文書が必要に応じてマークする特定の文書に関する情報。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">文書固有の値。各文書はその内容に応じて異なるメタデータ値をマークします。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: top;">文書Aの「著者」フィールド値は「張三」、文書Bの「著者」フィールド値は「李四」。</td>
        </tr>
    </tbody>
</table>

- **「ナレッジベース管理インターフェースでのメタデータフィールドの削除」「メタデータ編集ポップアップでの選択された文書のメタデータフィールドの削除」「文書詳細インターフェースでのメタデータフィールドの削除」には、どのような違いがありますか？**

<table style="width: 100%; border-collapse: collapse; background-color: #fff;">
    <thead>
        <tr style="background-color: #f9fafb;">
            <th style="padding: 12px; border: 1px solid #e5e7eb; width: 15%;">操作方法</th>
            <th style="padding: 12px; border: 1px solid #e5e7eb; width: 25%;">操作手順</th>
            <th style="padding: 12px; border: 1px solid #e5e7eb; width: 20%;">影響範囲</th>
            <th style="padding: 12px; border: 1px solid #e5e7eb; width: 20%;">結果</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">ナレッジベース管理インターフェースでメタデータフィールドを削除</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">ナレッジベース管理インターフェースで、特定のメタデータフィールドの右側にある削除アイコンをクリックして、そのフィールドを削除します。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">ナレッジベース管理リストからそのメタデータフィールドとそのすべてのフィールド値を完全に削除します。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">そのフィールドはナレッジベースから削除され、すべての文書内のそのフィールドとすべてのフィールド値も消えます。</td>
        </tr>
        <tr>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">メタデータ編集ポップアップで選択された文書のメタデータフィールドを削除</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">メタデータ編集ポップアップで、特定のメタデータフィールドの右側にある削除アイコンをクリックして、そのフィールドを削除します。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">選択された文書のそのフィールドとフィールド値のみを削除し、フィールド自体はナレッジベース管理リストに保持されます。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">選択された文書内のフィールドとフィールド値は削除されますが、フィールドはナレッジベース内に保持され、フィールド値カウントは数値上の変化が発生します。</td>
        </tr>
        <tr>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">文書詳細インターフェースでメタデータフィールドを削除</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">文書詳細インターフェースのメタデータ編集モードで、特定のメタデータフィールドの右側にある削除アイコンをクリックして、そのフィールドを削除します。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">その文書のそのフィールドとフィールド値のみを削除し、フィールド自体はナレッジベース管理リストに保持されます。</td>
            <td style="padding: 12px; border: 1px solid #e5e7eb; vertical-align: middle;">その文書内のフィールドとフィールド値は削除されますが、フィールドはナレッジベース内に保持され、フィールド値カウントは数値上の変化が発生します。</td>
        </tr>
    </tbody>
</table>