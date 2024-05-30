
# RiderとUnreal Engine: ビルドエラー解決ガイド

このガイドは、Riderを使用してUnreal Engineのビルドエラーを解決するための手順を記録したものです。主な解決内容には、`UnrealBuildTool.dll`が見つからない問題や無効な`MinimumiOSVersion`設定が含まれます。

## ビルドエラー解決手順

1. **.NET SDKのインストール確認**
   .NET SDKが正しくインストールされていることを確認します。
   ```sh
   dotnet --list-sdks
   ```
   期待される出力:
   ```plaintext
   6.0.423 [C:\Program Files\dotnet\sdk]
   ```

2. **`global.json`の設定**
   プロジェクトルートに`global.json`ファイルが存在し、正しい.NET SDKバージョンが設定されていることを確認します。
   ```json
   {
     "sdk": {
       "version": "6.0.423"
     }
   }
   ```

3. **Visual Studioのインストール確認**
   必要なワークロードがインストールされていることを確認します:
   - .NET desktop development
   - Game development with C++

4. **RiderのMSBuildバージョン設定**
   Riderで、`File > Settings > Build, Execution, Deployment > Toolset and Build`に移動し、Use MSBuild versionで正しいMSBuildのパスが設定されていることを確認します。

5. **NuGetパッケージのリストア**
   Riderで、`View > Tool Windows > NuGet`を開き、`Restore`をクリックして必要なパッケージをリストアします。

6. **プロジェクトのクリーンビルド**
   Riderで:
   - `Build > Clean Project`を選択
   - 次に、`Build > Rebuild Project`を選択

7. **環境変数の設定**
   必要に応じて、`UE_ENGINE_DIRECTORY`が設定されていることを確認します:
   - 変数名: `UE_ENGINE_DIRECTORY`
   - 変数値: `D:\Applications\Epic Games\UE_5.3\`

   またはシステム環境変数のパスに直接追加:
   - 変数値: `D:\Applications\Epic Games\UE_5.3\`

8. **UnrealBuildTool.dllの手動ビルド**
   管理者権限でコマンドプロンプトを開き、Unreal Engineのバッチファイルディレクトリに移動します:
   ```sh
   cd D:\Applications\Epic Games\UE_5.3\Engine\Build\BatchFiles
   ./RunUAT.bat BuildUBT
   ```
   ⚠︎私はこれを行っていません。必要があればやってください。

9. **UnrealBuildTool.dllの場所の確認**
    UnrealBuildTool.dllが実際に存在するか確認します。存在しない場合は、手動でビルドするか、Unreal Engineのインストールを修復する必要があります。
    Epic Games Launcherを開きます。
    ライブラリからUnreal Engineを見つけます。
    ⋮（三点リーダー）をクリックし、Verify（検証）を選択します。

10. **Minimum iOS Versionエラーの解決**
    プロジェクト設定で`MinimumiOSVersion`を更新します:
    - `Edit > Project Settings > Platforms > iOS`に移動
    - `Minimum iOS Version`を有効な値（例: `IOS_15`）に設定

    または、`Config`ファイルを更新します:
    ```ini
    [IOSRuntimeSettings]
    MinimumiOSVersion=IOS_15
    ```

## 成功例
1. **問題:** `UnrealBuildTool.dll`が見つからない、`MinimumiOSVersion`が無効といったビルドエラー。
2. **解決方法:** 上記の手順に従い、`MinimumiOSVersion`を`IOS_15`に修正。

これらの手順により、ビルドエラーが解決され、プロジェクトが問題なくビルドされました。
