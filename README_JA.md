<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143532659-5bb79d22-f21c-4abf-86e3-ea9789353f44.png" alt="Demo">
</p>

# NOVA Shader: Particle System用多機能シェーダ

[![license](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE.md)
[![license](https://img.shields.io/badge/PR-welcome-green.svg)](LICENSE.md)
[![license](https://img.shields.io/badge/Unity-2020.3-green.svg)](LICENSE.md)

**ドキュメント** ([English](README.md), [日本語](README_JA.md))
| **サンプル** ([English](Assets/Samples/README.md), [日本語](Assets/Samples/README_JA.md))
| **デモ** ([English](Assets/Demo/README.md), [日本語](Assets/Demo/README_JA.md))

*NOVA Shader*はUnityのUniversal Render Pipeline (URP)に対応した、Particle Systemのための多機能シェーダです。  
ビジュアルエフェクト制作でよく使われる汎用的な機能をまとめているので、効率的に高品質なエフェクトを作成できます。

<p align="center">
  <img width=600 src="https://user-images.githubusercontent.com/47441314/144193003-53bcaa8a-b9a2-4b79-a1de-aa7b001abdaa.gif" alt="Sample1">
</p>
<p align="center">
  <img width=600 src="https://user-images.githubusercontent.com/47441314/144192957-64e63c4a-3644-4a08-8134-dcbeb85d5493.gif" alt="Sample2"><br>
  <font color="grey">Author: </font><a href="https://twitter.com/Ugokashiya">@Ugokashiya</a>
</p>

特徴的な機能としてはフローマップ、Flip-Book（連番テクスチャアニメーション）、ディゾルブ、フェード、回転、アニメーション可能なTintマップ、エミッション、ディストーションなどがあります。

<p align="center">
  <img width=600 src="https://user-images.githubusercontent.com/47441314/143531706-7f0230bb-4e4f-41de-9dbf-1586f295225c.gif" alt="Features"><br>
  <font color="grey">特徴的な機能</a>
</p>

詳細については以下のドキュメントや[サンプル](Assets/Samples/README_JA.md)、[デモ](Assets/Demo/README_JA.md)を参照してください。

## 目次

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!-- param::title::詳細:: -->
<details>
<summary>詳細</summary>

- [セットアップ](#%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97)
    - [要件](#%E8%A6%81%E4%BB%B6)
    - [インストール](#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
- [Uber Unlitシェーダ](#uber-unlit%E3%82%B7%E3%82%A7%E3%83%BC%E3%83%80)
    - [Render Settings](#render-settings)
    - [Base Map](#base-map)
    - [Tint Color](#tint-color)
    - [Flow Map](#flow-map)
    - [Color Correction](#color-correction)
    - [Alpha Transition](#alpha-transition)
    - [Emission](#emission)
    - [Transparency](#transparency)
- [Distortionシェーダ](#distortion%E3%82%B7%E3%82%A7%E3%83%BC%E3%83%80)
    - [Render Settings](#render-settings-1)
    - [Distortion](#distortion)
    - [Flow Map](#flow-map-1)
    - [Alpha Transition](#alpha-transition-1)
    - [Transparency](#transparency-1)
- [Custom Vertex Streamsとの連携](#custom-vertex-streams%E3%81%A8%E3%81%AE%E9%80%A3%E6%90%BA)
    - [Custom Dataを設定](#custom-data%E3%82%92%E8%A8%AD%E5%AE%9A)
    - [Custom Vertex Streamsを設定する](#custom-vertex-streams%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)
    - [マテリアルプロパティを設定する](#%E3%83%9E%E3%83%86%E3%83%AA%E3%82%A2%E3%83%AB%E3%83%97%E3%83%AD%E3%83%91%E3%83%86%E3%82%A3%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)
- [Mesh GPU Instancingを使う](#mesh-gpu-instancing%E3%82%92%E4%BD%BF%E3%81%86)
    - [Mesh GPU Instancingを有効化する](#mesh-gpu-instancing%E3%82%92%E6%9C%89%E5%8A%B9%E5%8C%96%E3%81%99%E3%82%8B)
    - [Custom Vertex Streamsを設定する](#custom-vertex-streams%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B-1)
- [Litシェーダについて](#lit%E3%82%B7%E3%82%A7%E3%83%BC%E3%83%80%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
- [ライセンス](#%E3%83%A9%E3%82%A4%E3%82%BB%E3%83%B3%E3%82%B9)

</details>
<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## セットアップ

#### 要件
本ライブラリは以下の環境に対応しています。

* Unity 2020.3 以上
* Universal Render Pipeline
* Shader Model 3.5

なお、[Mesh GPU Instancing](https://docs.unity3d.com/Manual/PartSysInstancing.html)を使う場合にはShader Model 4.5が要件となります。  
また、`Mirror Sampling`プロパティを有効にする場合には、ハードウェアが[Inline Sampler States](https://docs.unity3d.com/Manual/SL-SamplerStates.html)に対応している必要があります。

また本ドキュメントでは、Universal Render Pipeline のセットアップが完了していることを前提としています。  
Universal Render Pipelineに関する詳細は[Unityのマニュアル](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@13.1/manual/)を参照してください。

#### インストール
インストールは以下の手順で行います。

1. Window > Package Manager を選択
2. 「+」ボタン > Add package from git URL を選択
3. 以下を入力してインストール
   * https://github.com/CyberAgentGameEntertainment/NovaShader.git?path=/Assets/Nova

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143533003-177a51fc-3d11-4784-b9d2-d343cc622841.png" alt="Package Manager">
</p>

あるいはPackages/manifest.jsonを開き、dependenciesブロックに以下を追記します。

```json
{
    "dependencies": {
        "jp.co.cyberagent.nova": "https://github.com/CyberAgentGameEntertainment/NovaShader.git?path=/Assets/Nova"
    }
}
```

バージョンを指定したい場合には以下のように記述します。

* https://github.com/CyberAgentGameEntertainment/NovaShader.git?path=/Assets/Nova#1.0.0

## Uber Unlitシェーダ
Uber Unlitシェーダはライティングが反映されない多機能シェーダです。
発光するエフェクトなど、ライティングが不要なパーティクルにはこのシェーダを使用します。

Uber Unlitシェーダを使用するには、マテリアルに `Nova/Particles/UberUnlit` シェーダをアサインします。  
Inspectorから設定可能な各プロパティの説明は以下の通りです。

#### Render Settings
Render Settingsではマテリアルの描画方法を制御できます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143199732-c5c72e4b-a3d1-48dd-84de-9aae264d1df4.png" alt="Render Settings"><br>
  <font color="grey">Render Settings</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Render Type</b></td><td>
<p>
描画方法を以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Opaque: 不透明描画</li>
<li>Cutout: 不透明描画で、透明部分のピクセルを破棄する</li>
<li>Transparent: 半透明描画（デフォルト）</li>
</ul>
</p>
<p>
Cutoutを選択した場合には、CutOffプロパティが表示されます。<br>
Transparentを選択した場合には、Blend Modeプロパティが表示されます。
</p>
</td></tr>
<tr><td></td><td colspan=2><b>CutOff</b></td><td>
<p>
<b>Render TypeをCutoutに設定した時のみ表示されます。</b>
</p>
<p>
半透明部分を切り取る際の閾値を制御します。<br>
この値が大きいほど、よりアルファ値が大きい部分まで切り取ります。
</p>
</td></tr>
<tr><td></td><td colspan=2><b>Blend Mode</b></td><td>
<p>
<b>Render TypeをTransparentに設定した時のみ表示されます。</b>
</p>
<p>
色の合成方法を以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Alpha: アルファブレンディング（デフォルト）</li>
<li>Additive: 加算</li>
<li>Multiply: 乗算</li>
</ul>
</p>
</td></tr>
<tr><td colspan="3"><b>Render Face</b></td><td>
<p>
描画する面を以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Front: 前面のみ描画（デフォルト）</li>
<li>Back: 背面のみ描画</li>
<li>Both: 両面を描画</li>
</ul>
</p>
</td></tr>
<tr><td colspan="3"><b>Render Priority</b></td><td>
<p>
描画の優先度を指定できます。<br>  
Render Typeが同じ場合、Render Priorityが小さいものから先に描画されます。
</p>
</td></tr>
<tr><td colspan="3"><b>Vertex Alpha Mode</b></td><td>
<p>
頂点カラーのアルファ値の用途を以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Alpha: アルファ値として扱う（デフォルト）</li>
<li>Transition Progress: Alpha Transition機能（後述）のProgressとして使う</li>
</ul>
</p>
</td></tr>
</tbody>
</table>


#### Base Map
Base Mapでは色のベースとなるテクスチャを制御できます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143205498-78b66ab9-3ea6-44ed-9a97-f5a00bda153e.png" alt="Base Map"><br>
  <font color="grey">Base Map</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Mode</b></td><td>
<p>
ベースマップのモードを以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Single Texture: 通常の2Dテクスチャ（デフォルト）</li>
<li>Flip Book: Flip-Bookアニメーション</li>
<li>Flip Book Blending: ブレンドありのFlip-Bookアニメーション</li>
</ul>
</p>
<p>
選択したモードによってテクスチャの型が変わります。
</p>
</td></tr>
<tr><td colspan="3"><b>Texture</b></td><td>
<p>
ベースマップを設定します。
</p>
<p>
ModeにFlip Bookを指定した場合には、<a href="https://docs.unity3d.com/2020.3/Documentation/Manual/class-Texture2DArray.html">Texture2DArray</a>を設定する必要があります。<br>
ModeにFlip Book Blendingを指定した場合には、<a href="https://docs.unity3d.com/2020.3/Documentation/Manual/class-Texture3D.html">Texture3D</a>を設定する必要があります。
</p>
</td></tr>
</td></tr>
<tr><td colspan="3"><b>Rotation</b></td><td>
<p>
ベースマップの回転度合いを設定します。
</p>
</td></tr>
<tr><td></td><td colspan=2><b>Offset</b></td><td>
<p>
ベースマップを回転する際の中心座標のオフセットです。
</p>
</td></tr>
<tr><td colspan="3"><b>Mirror Sampling</b></td><td>
<p>
チェックをつけるとテクスチャがミラーサンプリングされます。
</p>
</td></tr>
<tr><td colspan="3"><b>Flip-Book Progress</b></td><td>
<p>
<b>ModeをFlip BookあるいはFlip Book Blendingに設定した時のみ表示されます。</b>
</p>
<p>
Flip-BookあるいはFlip-Book Blendingの進行度を設定します。
</p>
</td></tr>
</tbody>
</table>

#### Tint Color
Tint Colorでは乗算色を指定できます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143205648-b669f20a-cc21-4a07-9d5c-3a18cd5cb085.png" alt="Tint Color"><br>
  <font color="grey">Tint Color</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Mode</b></td><td>
<p>
乗算色の適用範囲を以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>None: なし（デフォルト）</li>
<li>All: 全面に適用</li>
<li>Rim: リムに適用</li>
</ul>
</p>
</td></tr>
<tr><td></td><td colspan=2><b>Progress</b></td><td>
<p>
<b>ModeをRimに設定した時のみ表示されます。</b>
</p>
<p>
リムの範囲を設定します。<br>
この値が大きくすると、よりエッジに近い部分だけが着色されます。
</p>
</td></tr>
<tr><td></td><td colspan=2><b>Sharpness</b></td><td>
<p>
<b>ModeをRimに設定した時のみ表示されます。</b>
</p>
<p>
この値が大きいほどリムのエッジが鋭くなります。
</p>
</td></tr>
<tr><td></td><td colspan=2><b>Inverse</b></td><td>
<p>
<b>ModeをRimに設定した時のみ表示されます。</b>
</p>
<p>
リムの範囲を逆転させます。
</p>
</td></tr>
<tr><td colspan="3"><b>Color Mode</b></td><td>
<p>
乗算色の指定方法を以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Single Color: 単色（デフォルト）</li>
<li>Texture 2D: テクスチャで指定</li>
<li>Texture 3D: 3Dテクスチャで指定（アニメーション可能）</li>
</ul>
</p>
</td></tr>
<tr><td colspan=3><b>Color</b></td><td>
<p>
<b>Color ModeをSingle Colorに設定した時のみ表示されます。</b>
</p>
<p>
乗算色を指定します。
</p>
</td></tr>
<tr><td colspan=3><b>Texture</b></td><td>
<p>
<b>Color ModeをTexture 2DあるいはTexture 3Dに設定した時のみ表示されます。</b>
</p>
<p>
乗算色を表すテクスチャを設定します。
</p>
</td></tr>
<tr><td colspan=3><b>Progress</b></td><td>
<p>
<b>Color ModeをTexture 3Dに設定した時のみ表示されます。</b>
</p>
<p>
3Dテクスチャで指定した乗算色の進行度合いです。
</p>
</td></tr>
<tr><td colspan=3><b>Blend Rate</b></td><td>
<p>
乗算色の適用率です。
</p>
</td></tr>
</tbody>
</table>

#### Flow Map
Flow Mapを使うとベースマップを指定した方向に歪ませることができます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143205764-a4b1f81b-a62e-448c-ac86-0c888121e62e.png" alt="Flow Map"><br>
  <font color="grey">Flow Map</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Texture</b></td><td>
<p>
フローマップを設定します。フローマップの仕様は以下の通りです。
</p>
<p>
<ul>
<li>R値とG値に基づいて、サンプリングするUV値をずらします</li>
<li>値は0.5を基準として、小さいほどUV値がマイナス方向に、大きいほどプラス方向にずれます</li>
</ul>
色ではなく値としてテクスチャを使うので、テクスチャ設定のsRGB Colorのチェックを外す必要がある点に注意してください。
</p>
</td></tr>
<tr><td colspan=3><b>Intensity</b></td><td>
<p>
フローマップを適用する際の強度を設定します。
</p>
</td></tr>
</tbody>
</table>

#### Color Correction
Color Correctionはここまでの色を補正します。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143205890-207e62d5-174c-4f81-a1ea-a26bbc606769.png" alt="Color Correction"><br>
  <font color="grey">Color Correction</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Mode</b></td><td>
<p>
色調補正のモードを以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>None: 色調補正なし（デフォルト）</li>
<li>Greyscale: グレースケール化</li>
<li>Gradient Map: グラデーションマップを使って補正</li>
</ul>
</p>
</td></tr>
<tr><td><td colspan=2><b>Texture</b></td><td>
<p>
<b>ModeをGradient Mapに設定した時のみ表示されます。</b>
</p>
<p>
グラデーションマップを設定します。グラデーションマップの仕様は以下の通りです。
<ul>
<li>ここまでの色の輝度をグラデーションマップの色に置き換えます</li>
<li>輝度に応じてグラデーションマップをサンプリングする際のU値を変えます</li>
<li>従って横方向にグラデーションがかかったテクスチャを使う必要があります</li>
</ul>
</p>
</td></tr>
</tbody>
</table>

#### Alpha Transition
Alpha Transitionは徐々に消えていく表現に使用します。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143206056-2758e914-0f52-4472-bf67-c0c52098fc7d.png" alt="Alpha Transition"><br>
  <font color="grey">Alpha Transition</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Mode</b></td><td>
<p>
アルファトランジションのモードを以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>None: なし（デフォルト）</li>
<li>Fade: 薄くなって消えていくモード</li>
<li>Dissolve: 削れて消えていくモード</li>
</ul>
</p>
</td></tr>
<td colspan=3><b>Map Mode</b></td><td>
<p>
アルファトランジションマップのモードを以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Single Texture: 通常の2Dテクスチャ（デフォルト）</li>
<li>Flip Book: Flip-Bookアニメーション</li>
<li>Flip Book Blending: ブレンドありのFlip-Bookアニメーション</li>
</ul>
</p>
<p>
選択したモードによってテクスチャの型が変わります。
</p>
</td></tr>
<tr><td colspan="3"><b>Texture</b></td><td>
<p>
アルファトランジションマップを設定します。アルファトランジションマップの仕様は以下の通りです。
<ul>
<li>R値に基づいてアルファ値を変更します</li>
<li>R値が小さい部分ほど消えやすく、大きい部分ほど最後まで残ります</li>
</ul>
</p>
<p>
ModeにFlip Bookを指定した場合には、<a href="https://docs.unity3d.com/2020.3/Documentation/Manual/class-Texture2DArray.html">Texture2DArray</a>を設定する必要があります。<br>
ModeにFlip Book Blendingを指定した場合には、<a href="https://docs.unity3d.com/2020.3/Documentation/Manual/class-Texture3D.html">Texture3D</a>を設定する必要があります。
</p>
<p>
また色ではなく値としてテクスチャを使うので、テクスチャ設定のsRGB Colorのチェックを外す必要がある点に注意してください。
</p>
</td></tr>
<tr><td colspan="3"><b>Flip-Book Progress</b></td><td>
<p>
<b>Map ModeをFlip BookあるいはFlip Book Blendingに設定した時のみ表示されます。</b>
</p>
<p>
Flip-BookあるいはFlip-Book Blendingの進行度を設定します。
</p>
</td></tr>
<tr><td colspan="3"><b>Transition Progress</b></td><td>
<p>
トランジションの進行度を設定します。
</p>
</td></tr>
<tr><td colspan="3"><b>Edge Sharpness</b></td><td>
<p>
<b>ModeをTransitionに設定した時のみ表示されます。</b>
</p>
<p>
エッジの鋭さを設定します。
</p>
</td></tr>
</tbody>
</table>

#### Emission
Emissionは発光する表現のために使用します。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143206163-f6cd09cb-df77-4c52-ae96-da54ad4dfdec.png" alt="Emission"><br>
  <font color="grey">Emission</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Mode</b></td><td>
<p>
エミッションのモードを以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>None: なし（デフォルト）</li>
<li>All: 全面をエミッションさせます</li>
<li>By Texture: エミッションさせる範囲をテクスチャにより指定します</li>
<li>Edge: エッジ（アルファ値が0より大きく1より小さい部分）をエミッションさせます</li>
</ul>
</p>
</td></tr>
<td colspan=3><b>Map Mode</b></td><td>
<p>
<b>ModeをBy Textureに設定した時のみ表示されます。</b>
</p>
<p>
エミッションマップのモードを以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Single Texture: 通常の2Dテクスチャ（デフォルト）</li>
<li>Flip Book: Flip-Bookアニメーション</li>
<li>Flip Book Blending: ブレンドありのFlip-Bookアニメーション</li>
</ul>
</p>
<p>
選択したモードによってテクスチャの型が変わります。
</p>
</td></tr>
<tr><td colspan="3"><b>Texture</b></td><td>
<p>
<b>ModeをBy Textureに設定した時のみ表示されます。</b>
</p>
<p>
エミッションマップを設定します。エミッションマップの仕様は以下の通りです。
<ul>
<li>R値に基づいてエミッションのしやすさ変更します</li>
<li>R値が大きいほどエミッションしやすくなります</li>
</ul>
</p>
<p>
ModeにFlip Bookを指定した場合には、<a href="https://docs.unity3d.com/2020.3/Documentation/Manual/class-Texture2DArray.html">Texture2DArray</a>を設定する必要があります。<br>
ModeにFlip Book Blendingを指定した場合には、<a href="https://docs.unity3d.com/2020.3/Documentation/Manual/class-Texture3D.html">Texture3D</a>を設定する必要があります。
</p>
<p>
また色ではなく値としてテクスチャを使うので、テクスチャ設定のsRGB Colorのチェックを外す必要がある点に注意してください。
</p>
</td></tr>
<tr><td colspan="3"><b>Flip-Book Progress</b></td><td>
<p>
<b>ModeをBy Textureに設定し、Map ModeをFlip BookあるいはFlip Book Blendingに設定した時のみ表示されます。</b>
</p>
<p>
Flip-BookあるいはFlip-Book Blendingの進行度を設定します。
</p>
</td></tr>
<tr><td colspan="3"><b>Color Type</b></td><td>
<p>
エミッションの色を以下の選択肢から指定できます。
<ul>
<li>Color: 指定した単色をエミッション色として使用します</li>
<li>Base Color: ここまでのRGB値をエミッション色として使用します</li>
<li>Gradient Map: グラデーションマップを使ってエミッション色を指定します</li>
</ul>
Gradient MapはModeをBy TextureかEdgeに設定した時のみ選択可能です。
</p>
</td></tr>
<tr><td colspan="3"><b>Color</b></td><td>
<p>
<b>Color TypeをColorに設定した時のみ表示されます。</b>
</p>
<p>
エミッションの色をHDRカラーで指定します。
</p>
</td></tr>
<tr><td colspan="3"><b>Keep Edge Transparency</b></td><td>
<p>
<b>ModeをEdgeに設定した時のみ表示されます。</b>
</p>
<p>
チェックをつけると、エッジが透過しなくなります。
</p>
</td></tr>
<tr><td colspan="3"><b>Gradient Map</b></td><td>
<p>
<b>Color TypeをGradient Mapに設定した時のみ表示されます。</b>
</p>
<p>
グラデーションマップを設定します。グラデーションマップの仕様は以下の通りです。
<ul>
<li>ModeがBy Textureの場合: エミッションマップのR値をグラデーションマップのU値としてサンプリングします</li>
<li>ModeがEdgeの場合: アルファ値をグラデーションマップのU値としてサンプリングします</li>
<li>横方向にグラデーションがかかったテクスチャを使う必要があります</li>
</ul>
</p>
</td></tr>
<tr><td colspan="3"><b>Intensity</b></td><td>
<p>
エミッションの強さを設定します。
</p>
</td></tr>
</tbody>
</table>

#### Transparency
Transparencyは透明度を調整できます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143206260-12b123fe-858a-4770-96d1-a47cbab4079f.png" alt="Transparency"><br>
  <font color="grey">Transparency</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Rim</b></td><td>
<p>
チェックをつけるとリムを透過できます。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Progress</b></td><td>
<p>
透過の進行度合いを設定します。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Sharpness</b></td><td>
<p>
この値が大きいほど透過領域のエッジが鋭くなります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Inverse</b></td><td>
<p>
透過範囲を逆転させます。
</p>
</td></tr>
<tr><td colspan="3"><b>Luminance</b></td><td>
<p>
チェックをつけると輝度が小さい部分を透過させます。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Progress</b></td><td>
<p>
透過の進行度合いを設定します。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Sharpness</b></td><td>
<p>
この値が大きいほど透過領域のエッジが鋭くなります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Inverse</b></td><td>
<p>
透過範囲を逆転させます。
</p>
</td></tr>
<tr><td colspan="3"><b>Sort Particles</b></td><td>
<p>
チェックをつけるとソフトパーティクルを有効にします。<br>
この機能を使用するには、URPのDepth Texture設定が有効になっている必要があります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Intensity</b></td><td>
<p>
この値が大きいほど、透過する範囲が大きくなります。
</p>
</td></tr>
<tr><td colspan="3"><b>Depth Fade</b></td><td>
<p>
カメラに近い部分と遠い部分を透過させます。<br>
この機能を使用するには、URPのDepth Texture設定が有効になっている必要があります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Distance</b></td><td>
<p>
透過する範囲を設定します。<br>
カメラから見てNearより近い部分、Farより遠い部分が透過されます。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Width</b></td><td>
<p>
透過し始めてから完全に透過するまでの距離です。
</p>
</td></tr>
</tbody>
</table>

## Distortionシェーダ
Distortionは画面に対して歪み効果をかけるためのシェーダです。  
熱波など、歪み効果が必要なエフェクトにはこのシェーダを使用します。

Distortionシェーダを使用するには、マテリアルに `Nova/Particles/Distortion` シェーダをアサインします。  
Inspectorから設定可能な各プロパティの説明は以下の通りです。

#### Render Settings
Render Settingsではマテリアルの描画方法を制御できます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143212609-9a135eb7-d2d2-482c-bc79-73886852e92a.png" alt="Render Settings"><br>
  <font color="grey">Render Settings</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Render Face</b></td><td>
<p>
描画する面を以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>Front: 前面のみ描画（デフォルト）</li>
<li>Back: 背面のみ描画</li>
<li>Both: 両面を描画</li>
</ul>
</p>
</td></tr>
</tbody>
</table>

#### Distortion
Distortionでは歪ませ方を設定できます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143212824-353c2a08-f6c4-473f-8515-3eef8fbaf125.png" alt="Distortion"><br>
  <font color="grey">Distortion</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Texture</b></td><td>
<p>
ディストーションマップを設定します。ディストーションマップの仕様は以下の通りです。
</p>
<p>
<ul>
<li>R値とG値に基づいて画面を歪ませます</li>
<li>値は0.5を基準値として、基準値から離れるほど強く歪みます</li>
</ul>
色ではなく値としてテクスチャを使うので、テクスチャ設定のsRGB Colorのチェックを外す必要がある点に注意してください。
</p>
</td></tr>
<tr><td colspan=3><b>Intensity</b></td><td>
<p>
歪みの強さを設定します。
</p>
</td></tr><tr><td colspan="3"><b>Rotation</b></td><td>
<p>
ディストーションマップの回転度合いを設定します。
</p>
</td></tr>
<tr><td></td><td colspan=2><b>Offset</b></td><td>
<p>
ディストーションマップを回転する際の中心座標のオフセットです。
</p>
</td></tr>
<tr><td colspan="3"><b>Mirror Sampling</b></td><td>
<p>
チェックをつけるとテクスチャがミラーサンプリングされます。
</p>
</td></tr>
</tbody>
</table>

#### Flow Map
Flow Mapを使うとディストーションマップを指定した方向に歪ませることができます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143212922-b4c49a35-d078-4389-a192-5a6aaf5fba48.png" alt="Flow Map"><br>
  <font color="grey">Flow Map</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Texture</b></td><td>
<p>
フローマップを設定します。フローマップの仕様は以下の通りです。
</p>
<p>
<ul>
<li>R値とG値に基づいて、サンプリングするUV値をずらします</li>
<li>値は0.5を基準として、小さいほどUV値がマイナス方向に、大きいほどプラス方向にずれます</li>
</ul>
色ではなく値としてテクスチャを使うので、テクスチャ設定のsRGB Colorのチェックを外す必要がある点に注意してください。
</p>
</td></tr>
<tr><td colspan=3><b>Intensity</b></td><td>
<p>
フローマップを適用する際の強度を設定します。
</p>
</td></tr>
</tbody>
</table>

#### Alpha Transition
Alpha Transitionは徐々に消えていく表現に使用します。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143213099-a3ca1fb7-0078-4d67-bdb5-0b0d418a88e6.png" alt="Alpha Transition"><br>
  <font color="grey">Alpha Transition</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Mode</b></td><td>
<p>
アルファトランジションのモードを以下の選択肢から指定できます。
</p>
<p>
<ul>
<li>None: なし（デフォルト）</li>
<li>Fade: 薄くなって消えていくモード</li>
<li>Dissolve: 削れて消えていくモード</li>
</ul>
</p>
</td></tr>
<tr><td colspan="3"><b>Texture</b></td><td>
<p>
アルファトランジションマップを設定します。アルファトランジションマップの仕様は以下の通りです。
<ul>
<li>R値に基づいてアルファ値を変更します</li>
<li>R値が小さい部分ほど消えやすく、大きい部分ほど最後まで残ります</li>
</ul>
</p>
<p>
色ではなく値としてテクスチャを使うので、テクスチャ設定のsRGB Colorのチェックを外す必要がある点に注意してください。
</p>
</td></tr>
<tr><td colspan="3"><b>Progress</b></td><td>
<p>
トランジションの進行度を設定します。
</p>
</td></tr>
<tr><td colspan="3"><b>Edge Sharpness</b></td><td>
<p>
<b>ModeをTransitionに設定した時のみ表示されます。</b>
</p>
<p>
エッジの鋭さを設定します。
</p>
</td></tr>
</tbody>
</table>

#### Transparency
Transparencyは透明度を調整できます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143213193-3405c0c0-5812-4c41-be52-18b8c48ebd1c.png" alt="Transparency"><br>
  <font color="grey">Transparency</font>
</p>

<table width="100%">
<thead>
<tr><td colspan="3"><b>プロパティ名</b></td><td><b>説明</b></td></tr>
</thead>
<tbody>
<tr><td colspan="3"><b>Rim</b></td><td>
<p>
チェックをつけるとリムを透過できます。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Progress</b></td><td>
<p>
透過の進行度合いを設定します。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Sharpness</b></td><td>
<p>
この値が大きいほど透過領域のエッジが鋭くなります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Inverse</b></td><td>
<p>
透過範囲を逆転させます。
</p>
</td></tr>
<tr><td colspan="3"><b>Luminance</b></td><td>
<p>
チェックをつけると輝度が小さい部分を透過させます。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Progress</b></td><td>
<p>
透過の進行度合いを設定します。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Sharpness</b></td><td>
<p>
この値が大きいほど透過領域のエッジが鋭くなります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Inverse</b></td><td>
<p>
透過範囲を逆転させます。
</p>
</td></tr>
<tr><td colspan="3"><b>Sort Particles</b></td><td>
<p>
チェックをつけるとソフトパーティクルを有効にします。<br>
この機能を使用するには、URPのDepth Texture設定が有効になっている必要があります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Intensity</b></td><td>
<p>
この値が大きいほど、透過する範囲が大きくなります。
</p>
</td></tr>
<tr><td colspan="3"><b>Depth Fade</b></td><td>
<p>
カメラに近い部分と遠い部分を透過させます。<br>
この機能を使用するには、URPのDepth Texture設定が有効になっている必要があります。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Distance</b></td><td>
<p>
透過する範囲を設定します。<br>
カメラから見てNearより近い部分、Farより遠い部分が透過されます。
</p>
</td></tr>
<tr><td></td><td colspan="2"><b>Width</b></td><td>
<p>
透過し始めてから完全に透過するまでの距離です。
</p>
</td></tr>
</tbody>
</table>

## Custom Vertex Streamsとの連携
Particle SystemのCustom Vertex Streamsを使うと、マテリアルのプロパティを自由にアニメーションさせることができます。  
以下では例として、Custom Vertex Streamsを使ってテクスチャを回転させる手順を説明します。

#### Custom Dataを設定
まず[Particle SystemのCustom Data](https://docs.unity3d.com/2019.4/Documentation/Manual/PartSysCustomDataModule.html)を設定します。  
今回は以下のように、Custom1のXに0から始まり時間経過とともに1に移り変わる値を設定しました。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143419403-d5f33c24-6875-4e0e-bf05-c6ebdd94bb94.png" alt="Custom Data"><br>
  <font color="grey">Custom Data</font>
</p>

#### Custom Vertex Streamsを設定する
次にCustom Vertex Streamsを下図のように設定し、`Custom1.x`を`TEXCOORD1.x`に渡します。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143446418-0daf3b2e-7f21-4b0a-a78e-aac50770a186.png" alt="Custom Vertex Streams"><br>
  <font color="grey">Custom Vertex Streams</font>
</p>

#### マテリアルプロパティを設定する
次にマテリアルプロパティを設定します。  
今回は`TEXCOORD1.x`に渡された値を`Rotation`として設定したいため、下図のように`COORD 1X`を選択します。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143424542-61dc2d6b-402f-45d1-85bb-a2170e05643c.png" alt="Coord"><br>
  <font color="grey">Coord</font>
</p>

これでテクスチャが回転するようになりました。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143531888-b49c55e9-3df1-4dae-a0fe-d3e4a1638af2.gif" alt="Rotation"><br>
  <font color="grey">Rotation</font>
</p>

## Mesh GPU Instancingを使う
[Particle System Mesh GPU Instancing](https://docs.unity3d.com/Manual/PartSysInstancing.html)を使うと、パーティクルを効率的に描画できます。  
以下では本シェーダを使ったマテリアルに対して`Mesh GPU Instancing`を有効化する手順について説明します。

#### Mesh GPU Instancingを有効化する
`Mesh GPU Instancing`を使うには`Renderer`モジュールの`Render Mode`をMeshにする必要があります。  
またその上で`Enable Mesh GPU Instancing`にチェックを入れます。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143447533-0e03627f-9af0-43cd-bab1-254c78ea7f93.png" alt="Enable Mesh GPU Instancing"><br>
  <font color="grey">Enable Mesh GPU Instancing</font>
</p>

#### Custom Vertex Streamsを設定する
次にCustom Vertex Streamsを下図のように設定します。

<p align="center">
  <img width=500 src="https://user-images.githubusercontent.com/47441314/143448020-45beb08a-6795-4372-894a-c04e33a8029d.png" alt="Custom Vertex Streams"><br>
  <font color="grey">Custom Vertex Streams</font>
</p>

`Custom Data`の部分はNoiseなど他のモジュールの値を入れても問題ありませんが、  
必ず`INSTANCED1.xyzw`と`INSTANCED2.xyzw`の全てが過不足なく埋まるように設定してください。

以上で`Mesh GPU Instancing`の設定は完了です。

## Litシェーダについて
NOVA Shaderでは現状Unlitシェーダのみを提供しています。  
Litシェーダについては検討中の段階で、今後のアップデートで追加する予定です。

## ライセンス
本ソフトウェアはMITライセンスで公開しています。  
ライセンスの範囲内で自由に使っていただけますが、使用の際は以下の著作権表示とライセンス表示が必須となります。

* [LICENSE.md](LICENSE.md)

また、本ドキュメントの目次は以下のソフトウェアを使用して作成されています。

* [toc-generator](https://github.com/technote-space/toc-generator)

toc-generatorのライセンスの詳細は [Third Party Notices.md](Third%20Party%20Notices.md) を参照してください。