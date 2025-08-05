# 5.2 AI推論の可視化技術

## はじめに

AI生成コードの品質管理において最も重要なのは、AIがどの部分を「推測」したかを明確に把握することです。信号機システムを用いたAI推論の可視化技術により、効率的なレビューと高い品質保証を実現できます。

## 信号機システムの理論的背景

### 課題の特定

AI生成コードには以下の特性があります：

- **自動補完の広範囲性**: AIは明示されていない部分も自動的に補完
- **それっぽさの罠**: 生成内容が妥当に見えるが、実際の意図と異なる場合がある
- **推測根拠の不明確性**: どの情報に基づいて生成されたかが不明

### 解決アプローチ

信号機システムは、AI生成内容を以下の基準で分類し、レビューの優先度を明確化します：

```
🟢 青信号 → 🟡 黄信号 → 🔴 赤信号
  安全      注意       危険
```

## 信号機システムの詳細定義

### 🟢 青信号（高確信度・安全）

**定義：** 参照した元ファイルから明確に推測できる内容

**特徴：**
- 元の指示や仕様書に明記されている内容に基づく生成
- 既存コードのパターンに従った実装
- 明確な根拠がある実装判断

**具体例：**
```javascript
// 仕様書に「ユーザーIDは必須」と明記されている場合
function validateUser(userId) {
  if (!userId) {  // 🟢 仕様書から明確に導出
    throw new Error('User ID is required');
  }
}
```

**レビュー優先度：** 低
**確認ポイント：** 実装の正確性、パフォーマンス影響

### 🟡 黄信号（中確信度・注意）

**定義：** 参照した元ファイルにはないが、妥当だと思われる内容

**特徴：**
- AIの合理的な推測による補完
- 一般的なベストプラクティスに基づく実装
- ドメイン知識を活用した推論

**具体例：**
```javascript
// 仕様書に詳細がない場合のエラーハンドリング
function processData(data) {
  try {
    return transform(data);
  } catch (error) {  // 🟡 一般的だが確認が必要
    console.error('Data processing failed:', error);
    return null;
  }
}
```

**レビュー優先度：** 高
**確認ポイント：** 推測の妥当性、ビジネス要件との整合性

### 🔴 赤信号（要判断・危険）

**定義：** 参照した元ファイルになく、直接推測もできない内容

**特徴：**
- AIの独自判断による生成
- 組織固有の慣習やルールの想定
- 明確な根拠のない実装選択

**具体例：**
```javascript
// 組織のログ形式について情報がない場合
function logUserAction(action) {
  // 🔴 ログ形式は組織固有、要確認
  logger.info(`[AUDIT] User performed: ${action} at ${new Date().toISOString()}`);
}
```

**レビュー優先度：** 最高
**確認ポイント：** 組織ルールとの整合性、セキュリティ影響

## 実装方法とTODOファイル形式

### 標準TODOファイル形式

```markdown
## [ステップ名]結果TODO

### 🟢 高確信度項目
- [ ] [utils.js](./src/utils.js) の型定義が仕様書と一致することを確認
- [ ] [validation.js](./src/validation.js) の必須項目チェック実装の確認

### 🟡 中確信度項目
- [ ] [error-handler.js](./src/error-handler.js) のエラーレスポンス形式の妥当性確認
- [ ] [config.js](./src/config.js) のデフォルト値設定の組織ポリシー適合性

### 🔴 要判断項目
- [ ] 詳細確認: [logger.js](./src/logger.js) のログ出力形式が組織標準に準拠
- [ ] 詳細確認: [auth.js](./src/auth.js) のセッション管理方式の選択根拠
```

### プロンプトでの指示方法

**基本指示テンプレート：**
```markdown
## AI推論可視化指示

以下の作業を実行し、生成内容を信号機システムで分類してください：

**作業内容：**
[具体的なタスク内容]

**分類基準：**
- 🟢 青信号：参照ファイル（[ファイル名]）から明確に導出可能
- 🟡 黄信号：合理的推測だが参照ファイルに明記なし
- 🔴 赤信号：独自判断による生成（組織固有の内容等）

**出力ファイル：** `./todos/[ステップ名]-inference-check.md`

**出力フォーマット：**
各生成項目について信号機マークを付与し、TODO形式でチェック項目を作成
```

### 参照元ファイルの管理方法

**ファイル関係性の記録：**
```markdown
## 参照ファイル管理

**主要参照（Primary References）：**
- [`requirements.md`](./docs/requirements.md) - 基本要件定義
- [`api-spec.yaml`](./docs/api-spec.yaml) - API仕様

**補助参照（Secondary References）：**
- [`existing-code/`](./src/existing/) - 既存実装パターン
- [`config-samples/`](./config/) - 設定ファイル例

**外部参照（External References）：**
- 技術ドキュメント（フレームワーク公式）
- 業界標準（RFC、W3C等）

**推論の根拠追跡：**
- 🟢項目 → 主要参照に明記
- 🟡項目 → 補助参照＋一般知識
- 🔴項目 → 根拠不明・独自判断
```

## チェック優先度の設定

### 優先度マトリックス

| 信号 | 影響度・高 | 影響度・中 | 影響度・低 |
|------|-----------|-----------|-----------|
| 🔴 赤信号 | **最優先** | 高優先 | 中優先 |
| 🟡 黄信号 | 高優先 | 中優先 | 低優先 |
| 🟢 青信号 | 中優先 | 低優先 | **後回し** |

### 影響度評価基準

**影響度・高：**
- セキュリティに関わる実装
- データの整合性に影響
- システム全体の動作に影響

**影響度・中：**
- 特定機能の動作に影響
- ユーザー体験に影響
- パフォーマンスに影響

**影響度・低：**
- ログ出力やコメント
- 内部的な変数名
- 補助的な機能

### 実践的なチェック順序

1. **🔴×影響度・高** - 即座に確認・修正
2. **🔴×影響度・中** および **🟡×影響度・高** - 次回作業開始前に確認
3. **その他の🔴項目** - 実装完了前に必ず確認
4. **🟡項目** - レビュー時に確認
5. **🟢項目** - 最終チェック時に確認

## 実際の運用例とケーススタディ

### ケーススタディ1: REST API実装

**シナリオ：** ユーザー管理APIの実装
**参照ファイル：** `user-api-spec.yaml`, `existing-user-model.js`

**AI生成結果の分類：**

```javascript
// 🟢 API仕様書に明記されたエンドポイント
app.post('/api/users', async (req, res) => {
  
  // 🟡 一般的なバリデーションだが詳細は仕様書になし
  if (!req.body.email || !req.body.password) {
    return res.status(400).json({ error: 'Email and password required' });
  }
  
  // 🔴 ハッシュ化アルゴリズムが組織固有ポリシーに依存
  const hashedPassword = bcrypt.hashSync(req.body.password, 12);
  
  // 🟢 既存モデルのパターンに従った実装
  const user = new User({
    email: req.body.email,
    password: hashedPassword
  });
});
```

**生成されたTODO：**
```markdown
## API実装結果TODO

### 🟢 高確信度項目
- [ ] [user-controller.js](./src/controllers/user.js) のエンドポイント定義が仕様書と一致
- [ ] [user-model.js](./src/models/user.js) の既存パターン踏襲の確認

### 🟡 中確信度項目
- [ ] [validation.js](./src/middleware/validation.js) のエラーメッセージ形式の妥当性
- [ ] [user-controller.js](./src/controllers/user.js) のステータスコード選択の確認

### 🔴 要判断項目
- [ ] 詳細確認: [auth.js](./src/utils/auth.js) のbcryptソルトラウンド数が組織ポリシーに準拠
```

### ケーススタディ2: テストケース生成

**シナリオ：** 上記APIのテストケース作成
**参照ファイル：** `user-api-spec.yaml`, `existing-test-patterns.js`

**分類結果：**
```javascript
describe('User API', () => {
  // 🟢 仕様書に明記されたテストケース
  it('should create user with valid email and password', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: 'password123' });
    
    expect(response.status).toBe(201);  // 🟢 仕様書通り
  });
  
  // 🟡 一般的なエッジケース（仕様書に明記なし）
  it('should reject invalid email format', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'invalid-email', password: 'password123' });
    
    expect(response.status).toBe(400);  // 🟡 推測による
  });
  
  // 🔴 組織固有のセキュリティ要件による推測
  it('should enforce password complexity requirements', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: '123' });
    
    expect(response.status).toBe(400);  // 🔴 組織ポリシー依存
  });
});
```

## 効果測定と改善方法

### 効果測定指標

**定量指標：**
- レビュー時間の短縮率
- バグ発見率の向上
- 修正回数の減少

**定性指標：**
- レビューの効率性向上
- 重要な問題の見落とし防止
- 開発者の安心感向上

### 運用データの例

```markdown
## 信号機システム導入効果（1ヶ月間）

**従来のレビュー：**
- 平均レビュー時間：45分/機能
- バグ発見率：約60%
- レビューでの見落とし：月3-4件

**信号機システム導入後：**
- 平均レビュー時間：25分/機能（44%短縮）
- バグ発見率：約85%（25%向上）
- レビューでの見落とし：月1件以下

**🔴項目の典型的な問題：**
- セキュリティ関連：40%
- 組織ポリシー違反：35%
- 設定・環境依存：25%
```

### 継続的改善のアプローチ

**1. 分類精度の向上：**
```markdown
## 分類基準の改善ログ

**Week 1-2：**
- 問題：ログ形式の分類が曖昧
- 改善：組織固有項目のチェックリスト作成

**Week 3-4：**
- 問題：エラーハンドリングの分類が不安定
- 改善：エラーハンドリングパターンの明文化

**Month 2：**
- 問題：新技術スタックでの分類困難
- 改善：技術スタック別ガイドライン作成
```

**2. プロンプト最適化：**
```markdown
## プロンプト改善サイクル

**改善前の課題：**
- 🔴項目の検出率が70%程度
- 分類に一貫性がない

**改善内容：**
- 組織固有項目の明確なリスト提供
- 判断基準の具体例を豊富に追加

**改善後の効果：**
- 🔴項目の検出率が90%以上に向上
- 分類の一貫性が大幅に改善
```

## 実践的な導入ステップ

### Step 1: 基本システムの構築

```bash
# プロジェクト構造の準備
mkdir -p todos
mkdir -p docs/inference-guides

# 基本テンプレートの作成
cat > docs/inference-guides/classification-template.md << 'EOF'
## AI推論分類テンプレート

### 🟢 青信号の判定基準
- 参照ファイル「[ファイル名]」に明記されている内容
- 既存コードの確立されたパターンに従う実装

### 🟡 黄信号の判定基準  
- 一般的なベストプラクティスに基づく実装
- 技術的に妥当だが参照ファイルに明記なし

### 🔴 赤信号の判定基準
- 組織固有のポリシーや慣習に依存
- 明確な根拠なしの独自判断
EOF
```

### Step 2: 組織固有ルールの明文化

```markdown
## 組織固有チェックポイント

**セキュリティ関連：**
- [ ] パスワードハッシュ化アルゴリズムと強度
- [ ] セッション管理方式
- [ ] API認証方式

**ログ・監査関連：**
- [ ] ログ出力形式とレベル
- [ ] 監査ログの出力項目
- [ ] ログ保存期間とローテーション

**コーディング規約：**
- [ ] 命名規則（変数、関数、クラス）
- [ ] エラーハンドリングパターン
- [ ] コメント記述ルール
```

### Step 3: チーム運用の確立

```markdown
## チーム運用ルール

**分類作業の担当：**
- AI実行者が初期分類
- レビューアが分類の妥当性確認

**チェック作業の分担：**
- 🔴項目：シニアエンジニアが確認
- 🟡項目：チーム内でペアレビュー  
- 🟢項目：自動テスト＋軽微な確認

**知見の蓄積：**
- 週1回の分類基準見直し会議
- 誤分類パターンの共有
- 改善事例の文書化
```

## トラブルシューティング

### よくある問題と対処法

**問題1: 分類が一貫しない**

```markdown
**症状：** 同じような内容でも分類結果が異なる
**原因：** 分類基準が曖昧、プロンプトの指示が不明確
**対処法：**
1. 組織固有項目のより具体的なリスト作成
2. 過去の分類例をプロンプトに含める
3. 判断迷い事項の記録と基準化
```

**問題2: 🔴項目の見逃し**

```markdown
**症状：** 重要な組織固有項目が🟡や🟢に分類される
**原因：** AIが組織のコンテキストを理解していない
**対処法：**
1. 組織固有項目の明示的なリスト提供
2. 「疑わしい場合は🔴に分類」の原則設定
3. レビューアによる分類妥当性チェック
```

**問題3: TODO項目が多すぎる**

```markdown
**症状：** 生成されるTODO項目数が実行可能な範囲を超える
**原因：** 分類が細かすぎる、影響度評価が甘い
**対処法：**
1. 類似項目のグルーピング
2. 影響度評価の厳格化
3. 「重要度×緊急度」マトリックスの導入
```

## 実践演習

### 演習1: 分類基準の作成

以下のコードスニペットを信号機システムで分類してください：

```javascript
function authenticateUser(username, password) {
  // ケース1: ユーザー存在チェック
  const user = await User.findOne({ username });
  if (!user) {
    return { success: false, message: 'User not found' };
  }
  
  // ケース2: パスワード照合
  const isValid = await bcrypt.compare(password, user.hashedPassword);
  if (!isValid) {
    return { success: false, message: 'Invalid password' };
  }
  
  // ケース3: JWTトークン生成
  const token = jwt.sign(
    { userId: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
  
  // ケース4: ログ出力
  console.log(`User ${username} authenticated successfully at ${new Date().toISOString()}`);
  
  return { success: true, token };
}
```

**参照ファイル：** `auth-spec.md`（基本認証フローのみ記載）

### 演習2: TODO項目の優先度設定

上記の分類結果に基づいて、TODO項目を優先度順に並べてください。

## まとめ

AI推論の可視化技術により、以下の効果を実現できます：

1. **効率的なレビュー**: 重要な箇所に集中した品質チェック
2. **リスク軽減**: 高リスクな推測部分の確実な発見
3. **知見蓄積**: 組織固有の判断基準の明文化と共有
4. **継続改善**: データに基づく分類基準とプロンプトの最適化

信号機システムは、AITDDにおけるヒューマンファクターとAI支援のバランスを取る重要な技術です。次のセクションでは、これらの技術を活用した継続的改善とプロンプト最適化について学習します。
