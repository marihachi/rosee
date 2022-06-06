## parse API
入力文字列からノードツリーを生成します。  
全てのMFM構文を利用可能です。  

例:  
```ts
const nodes = mfm.parse('hello $[tada world]');
console.log(JSON.stringify(nodes));
// => [{"type":"text","props":{"text":"hello "}},{"type":"fn","props":{"name":"tada","args":{}},"children":[{"type":"text","props":{"text":"world"}}]}]
```

### 利用可能なMFM関数のリストを設定する
MFM関数の名前をホワイトリストに登録して、登録されたMFM関数以外を通常のテキストノードとして解釈するように設定できます。  
デフォルトではすべてのMFM関数名を受け入れるように設定されています。  

例:  
```ts
const nodes = mfm.parse('hello $[tada world]', { fnNameList: ['tada', 'spin'] });
console.log(JSON.stringify(nodes));
// => [{"type":"text","props":{"text":"hello "}},{"type":"fn","props":{"name":"tada","args":{}},"children":[{"type":"text","props":{"text":"world"}}]}]
```

```ts
const nodes = mfm.parse('hello $[pope world]', { fnNameList: ['tada', 'spin'] });
console.log(JSON.stringify(nodes));
// => [{"type":"text","props":{"text":"hello $[pope world]"}}]
```

### 最大のネストの深さを変更する
デフォルトで20に設定されています。  

例:  
```ts
const nodes = mfm.parse('**<s>cannot nest</s>**', { nestLimit: 1 });
console.log(JSON.stringify(nodes));
// => [{"type":"bold","children":[{"type":"text","props":{"text":"<s>cannot nest</s>"}}]}]
```

## parseSimple API
入力文字列からノードツリーを生成します。  
絵文字コードとUnicode絵文字を利用可能です。  

例:  
```ts
const nodes = mfm.parseSimple('Hello :surprised_ai:');
console.log(JSON.stringify(nodes));
// => [{"type":"text","props":{"text":"Hello "}},{"type":"emojiCode","props":{"name":"surprised_ai"}}]
```

## toString API
ノードツリーからMFM文字列を生成します。

例:  
```ts
const nodes = mfm.parse('hello $[tada world]');
const output = mfm.toString(nodes);
console.log(output); // => "hello $[tada world]"
```
※元の文字列とtoString APIで出力される文字列の同一性は保障されません。

## inspect API
ノードツリーの全ノードに指定された関数を適用します。  

例:  
```ts
mfm.inspect(nodes, node => {
  if (node.type == 'text') {
    node.props.text = node.props.text.replace(/Good morning/g, 'Hello');
  }
});
```

## extract API
ブール値を返す関数を渡してノードを抽出します。  
このAPIはノードツリーを再帰的に探索します。  

例:  
```ts
mfm.extract(nodes, (node) => {
  return (node.type === 'emojiCode');
});
```
