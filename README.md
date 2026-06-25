# build-hsp

HSP3スクリプトを`hsc3_make`経由でスタンドアロン実行ファイルにビルドし、生成された実行ファイルのパスを出力するcomposite action(Gitea Actions / GitHub Actions互換)。`hsp-home`は[`kijuky/setup-hsp`](https://github.com/kijuky/setup-hsp)の出力をそのまま渡すことを想定している。

**Inputs**

| 名前 | 必須 | 説明 |
|---|---|---|
| `hsp-home` | ✓ | 展開済みHSP3インストールへのパス(`setup-hsp`の`hsp-home`出力) |
| `script` | ✓ | ビルド対象のHSP3スクリプトへのパス(例: `"main.hsp"`) |

**Outputs**

| 名前 | 説明 |
|---|---|
| `exe-path` | 生成された実行ファイルへのパス |

```yaml
- name: Setup SDK
  id: setup-sdk
  uses: kijuky/setup-hsp@<commit-sha>
  with:
    hsp-version: "3.6"

- name: Build app.exe
  id: build
  uses: kijuky/build-hsp@<commit-sha>
  with:
    hsp-home: ${{ steps.setup-sdk.outputs.hsp-home }}
    script: main.hsp

- name: Create release
  uses: softprops/action-gh-release@<commit-sha> # vX.Y.Z
  with:
    files: ${{ steps.build.outputs.exe-path }}
```

## ピン留め規約

サプライチェーン攻撃対策として、このアクションを参照する側は**タグではなくコミットSHAで固定**すること(`actions/checkout`等の外部アクションと同じ規約)。バージョンの目印として、SHAの末尾にタグ名やバージョンをコメントで残す。

```yaml
uses: kijuky/build-hsp@1234567890abcdef1234567890abcdef12345678 # v1.0.0
```
