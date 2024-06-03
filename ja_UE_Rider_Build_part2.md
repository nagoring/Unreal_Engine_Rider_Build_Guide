
# Unreal EngineとRiderのビルドエラー解決手順

このドキュメントは、Unreal Engineプロジェクトのビルドエラーを解決するために行った手順をまとめたものです。

## 問題の概要

Unreal Engineエディタでプロジェクトをビルドしようとすると、以下のようなエラーが発生しました：

```
Could not spawn process D:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.36.32532\bin\Hostx64\x64\link.exe. Error: 267
Failed to link patch (0.000s) (Exit code: 0xFFFFFFFF)
---------- Finished (0.000s) ----------
```

## 解決手順

### 1. Visual Studioのインストールと設定

- Visual Studioのインストールディレクトリを確認し、必要なコンポーネントがインストールされていることを確認しました。
- Visual Studio Installerを使用して、以下のワークロードがインストールされていることを確認しました：
  - `.NET Desktop Development`
  - `Desktop Development with C++`
  - `C++によるゲーム開発`

### 2. 環境変数の設定

- 環境変数 `Path` に以下のディレクトリを追加しました：
  ```
  D:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.36.32532\bin\Hostx64\x64
  ```
- 環境変数の変更が反映されるようにシステムを再起動しました。

### 3. UnrealBuildToolのビルド

#### Visual Studioを使用してビルドする場合：

1. Visual Studioで `UnrealBuildTool.sln` ファイルを開く：
   ```
   D:\Applications\EpicGames\UE_5.3\Engine\Source\Programs\UnrealBuildTool\UnrealBuildTool.sln
   ```

2. ソリューションエクスプローラーで `UnrealBuildTool` プロジェクトを右クリックし、「ビルド」を選択。

#### コマンドラインを使用してビルドする場合：

1. コマンドプロンプトを管理者として実行。
2. 以下のコマンドを実行：
   ```shell
   cd "D:/Applications/EpicGames/UE_5.3/Engine/Source/Programs/UnrealBuildTool/"
   "C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\amd64\MSBuild.exe" UnrealBuildTool.sln /p:Configuration=Development
   ```

### 4. Riderの設定変更

Unreal Engineエディタの設定で、ソースコードエディタを `Rider UProject` に変更しました。

1. Unreal Engineエディタを開く：
   - `Edit` -> `Editor Preferences` に移動。

2. `General -> Source Code` の `Source Code Editor` を `Rider UProject` に設定。

### 5. 再度ビルドの実行

上記の設定を行った後、Unreal Engineエディタでプロジェクトのビルドを再試行し、エラーが解決されました。

### 6. Intel VTune Profilerのインストール

一部のエラーはIntel VTune Profilerの必要なDLLファイルが存在しないことが原因であるため、Intel VTune Profilerをインストールしました。

1. **Intel VTune Profilerのダウンロード**:
   - [Intel VTune Profiler ダウンロードページ](https://www.intel.com/content/www/us/en/developer/tools/oneapi/vtune-profiler.html) から最新バージョンをダウンロード。

2. **インストーラーの実行**:
   - ダウンロードした `bootstrapper.exe` ファイルを実行し、インストールウィザードに従ってインストール。

3. **必要なDLLファイルの確認**:
   - インストール後、`VtuneApi.dll` と `VtuneApi32e.dll` が正しくインストールされていることを確認。

### 7. システムファイルの整合性チェック

システムファイルが破損している可能性があるため、システムファイルチェッカーを使用してシステムファイルの整合性を確認しました。

1. **システムファイルチェッカーの実行**:
   - コマンドプロンプトを管理者として実行し、以下のコマンドを入力:
     ```shell
     sfc /scannow
     ```

2. **スキャンが完了するのを待つ**:
   - スキャンが完了するまで待ちます。完了後、再度インストーラーを実行して問題が解決したか確認します。

### 8. エラーのトラブルシューティング

ビルドエラーの原因を特定するために、詳細なログ情報を確認しました。

1. **ビルドログの確認**:
   - `D:/Documents/UnrealProjects/CoinDorobo1_Cplus/Saved/Logs` フォルダ内のログファイルを確認し、詳細なエラーメッセージを特定しました。

### 9. ビルドオプションの調整

ビルドオプションを調整し、詳細なログを有効にしてエラーの原因を特定しました。

1. **詳細なログの有効化**:
   - ビルドコマンドに `-verbose` オプションを追加し、詳細なログを取得:
     ```shell
     D:/Applications/EpicGames/UE_5.3/Engine/Build/BatchFiles/Build.bat CoinDorobo1_CplusEditor Win64 Development -Project="D:/Documents/UnrealProjects/CoinDorobo1_Cplus/CoinDorobo1_Cplus.uproject" -WaitMutex -FromMsBuild -verbose
     ```

### 10. ビルド成功後の確認

これらの手順をすべて実行した後、Unreal Engineプロジェクトのビルドが成功し、エラーが解消されました。

## 参考リンク

- [Intel VTune Profiler ダウンロードページ](https://www.intel.com/content/www/us/en/developer/tools/oneapi/vtune-profiler.html)
- [Qiitaの記事](https://qiita.com/_akoto_/items/f91a9e52d6e1f904bb78)
- [link.exe Error](https://forums.unrealengine.com/t/link-fatal-error-lnk1181-cannot-open-input-file/1151583/7)


