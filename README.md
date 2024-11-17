
![](https://img2024.cnblogs.com/blog/468667/202411/468667-20241115210016306-1289457179.gif)
 


【引言】（完整代码在最后面）


本文将介绍如何在鸿蒙NEXT中创建一个自定义的“太极Loading”组件，为你的应用增添独特的视觉效果。


【环境准备】


电脑系统：windows 10


开发工具：DevEco Studio NEXT Beta1 Build Version: 5\.0\.3\.806


工程版本：API 12


真机：mate60 pro


语言：ArkTS、ArkUI


【项目分析】


1\. 组件结构


我们将创建一个名为 TaiChiLoadingProgress 的自定义组件，它将模拟太极图的旋转效果，作为加载动画展示给用户。组件的基本结构如下：



[?](https://github.com)

| 12345678 | `@Component``struct TaiChiLoadingProgress {``@Prop taiChiWidth: number = 400``@Prop @Watch(``'animationCurveChanged'``) animationCurve: Curve = Curve.Linear``@State angle: number = 0``@State cellWidth: number = 0``...``}` |
| --- | --- |



2\. 绘制太极图案


使用鸿蒙NEXT提供的UI组件，如 Rect 和 Circle，构建太极图的黑白两部分。关键在于利用 rotate 方法实现太极图的旋转效果。



[?](https://github.com)

| 12345678910111213141516171819202122232425262728293031323334353637383940 | `build() {``Stack() {``Stack() {``// 黑色半圆背景``Stack() {``Rect().width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth / 2}px`).backgroundColor(Color.Black)``}.width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth}px`).rotate({ angle: -90 }).align(Alignment.Top)``// 大黑球 上``Stack() {``Circle().width(`${``this``.cellWidth / 2}px`).height(`${``this``.cellWidth / 2}px`).fill(Color.Black)``Circle().width(`${``this``.cellWidth / 8}px`).height(`${``this``.cellWidth / 8}px`).fill(Color.White)``}.width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth}px`).align(Alignment.Top)``// 大白球 下``Stack() {``Circle().width(`${``this``.cellWidth / 2}px`).height(`${``this``.cellWidth / 2}px`).fill(Color.White)``Circle().width(`${``this``.cellWidth / 8}px`).height(`${``this``.cellWidth / 8}px`).fill(Color.Black)``}.width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth}px`).align(Alignment.Bottom)``}``.width(`${``this``.cellWidth}px`)``.height(`${``this``.cellWidth}px`)``.borderWidth(1)``.borderColor(Color.Black)``.borderRadius(``'50%'``)``.backgroundColor(Color.White)``.clip(``true``)``.rotate({``angle:` `this``.angle``})``.onVisibleAreaChange([0.0, 1.0], (isVisible: boolean, currentRatio: number) => {``if` `(isVisible && currentRatio >= 1.0) {``this``.startAnim()``}``if` `(!isVisible && currentRatio <= 0.0) {``this``.endAnim()``}``})``}``.width(`${``this``.taiChiWidth}px`)``.height(`${``this``.taiChiWidth}px`)``}` |
| --- | --- |



3\. 动画实现


通过 animateTo 方法设置太极图的旋转动画，可以自定义动画曲线以实现不同的动画效果。



[?](https://github.com):[FlowerCloud机场](https://hanlianfangzhi.com)

| 1234567891011121314151617 | `startAnim() {``animateTo({``duration: 2000,``iterations: -1,``curve:` `this``.animationCurve``}, () => {``this``.angle = 360 * 2``})``}` `endAnim() {``animateTo({``duration: 0``}, () => {``this``.angle = 0``})``}` |
| --- | --- |



【完整代码】



[?](https://github.com)

| 123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197 | `@Component``struct TaiChiLoadingProgress {``@Prop taiChiWidth: number = 400``@Prop @Watch(``'animationCurveChanged'``) animationCurve: Curve = Curve.Linear``@State angle: number = 0``@State cellWidth: number = 0` `animationCurveChanged() {``this``.endAnim()``this``.startAnim()``}` `startAnim() {``animateTo({``duration: 2000,``iterations: -1,``curve:` `this``.animationCurve``}, () => {``this``.angle = 360 * 2``})``}` `endAnim() {``animateTo({``duration: 0``}, () => {``this``.angle = 0``})``}` `aboutToAppear(): void {``this``.cellWidth =` `this``.taiChiWidth / 2``}` `build() {``Stack() {``Stack() {``//黑色 半圆 背景``Stack() {``Rect().width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth / 2}px`).backgroundColor(Color.Black)``}.width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth}px`).rotate({ angle: -90 }).align(Alignment.Top)` `//大黑球 上``Stack() {``Stack() {``Circle().width(`${``this``.cellWidth / 2}px`).height(`${``this``.cellWidth / 2}px`).fill(Color.Black)``Circle().width(`${``this``.cellWidth / 8}px`).height(`${``this``.cellWidth / 8}px`).fill(Color.White)``}``}.width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth}px`).align(Alignment.Top)` `//大白球 下``Stack() {``Stack() {``Circle().width(`${``this``.cellWidth / 2}px`).height(`${``this``.cellWidth / 2}px`).fill(Color.White)``Circle().width(`${``this``.cellWidth / 8}px`).height(`${``this``.cellWidth / 8}px`).fill(Color.Black)``}``}.width(`${``this``.cellWidth}px`).height(`${``this``.cellWidth}px`).align(Alignment.Bottom)` `}``.width(`${``this``.cellWidth}px`)``.height(`${``this``.cellWidth}px`)``.borderWidth(1)``.borderColor(Color.Black)``.borderRadius(``'50%'``)``.backgroundColor(Color.White)``.clip(``true``)``.rotate({``angle:` `this``.angle``})``.onVisibleAreaChange([0.0, 1.0], (isVisible: boolean, currentRatio: number) => {``console.info(``'Test Row isVisible:'` `+ isVisible +` `', currentRatio:'` `+ currentRatio)``if` `(isVisible && currentRatio >= 1.0) {``console.info(``'Test Row is fully visible.'``)``this``.startAnim()``}` `if` `(!isVisible && currentRatio <= 0.0) {``console.info(``'Test Row is completely invisible.'``)``this``.endAnim()``}``})``}``.width(`${``this``.taiChiWidth}px`)``.height(`${``this``.taiChiWidth}px`)``}``}` `@Entry``@Component``struct Page08 {``@State loadingWidth: number = 150``@State isShowLoading: boolean =` `true``;``@State animationCurve: Curve = Curve.Linear` `build() {``Column({ space: 20 }) {` `Text(``'官方Loading组件'``)``Column() {``LoadingProgress().width(``this``.loadingWidth)``.visibility(``this``.isShowLoading ? Visibility.Visible : Visibility.None)``}.height(``this``.loadingWidth).width(``this``.loadingWidth)` `Text(``'自定义太极Loading组件'``)``Column() {``TaiChiLoadingProgress({ taiChiWidth: vp2px(``this``.loadingWidth), animationCurve:` `this``.animationCurve })``.visibility(``this``.isShowLoading ? Visibility.Visible : Visibility.Hidden)``}.height(``this``.loadingWidth).width(``this``.loadingWidth)` `Row() {``Flex({ wrap: FlexWrap.Wrap }) {``Text(``'显示/隐藏'``)``.textAlign(TextAlign.Center)``.width(``'200lpx'``)``.height(``'200lpx'``)``.margin(``'10lpx'``)``.backgroundColor(Color.Black)``.borderRadius(5)``.backgroundColor(Color.Orange)``.fontColor(Color.White)``.clickEffect({ level: ClickEffectLevel.LIGHT })``.onClick(() => {``this``.isShowLoading = !``this``.isShowLoading``})``Text(``'Linear动画'``)``.textAlign(TextAlign.Center)``.width(``'200lpx'``)``.height(``'200lpx'``)``.margin(``'10lpx'``)``.backgroundColor(Color.Black)``.borderRadius(5)``.backgroundColor(Color.Orange)``.fontColor(Color.White)``.clickEffect({ level: ClickEffectLevel.LIGHT })``.onClick(() => {``this``.animationCurve = Curve.Linear``})``Text(``'FastOutLinearIn动画'``)``.textAlign(TextAlign.Center)``.width(``'200lpx'``)``.height(``'200lpx'``)``.margin(``'10lpx'``)``.backgroundColor(Color.Black)``.borderRadius(5)``.backgroundColor(Color.Orange)``.fontColor(Color.White)``.clickEffect({ level: ClickEffectLevel.LIGHT })``.onClick(() => {``this``.animationCurve = Curve.FastOutLinearIn``})``Text(``'EaseIn动画'``)``.textAlign(TextAlign.Center)``.width(``'200lpx'``)``.height(``'200lpx'``)``.margin(``'10lpx'``)``.backgroundColor(Color.Black)``.borderRadius(5)``.backgroundColor(Color.Orange)``.fontColor(Color.White)``.clickEffect({ level: ClickEffectLevel.LIGHT })``.onClick(() => {``this``.animationCurve = Curve.EaseIn``})``Text(``'EaseOut动画'``)``.textAlign(TextAlign.Center)``.width(``'200lpx'``)``.height(``'200lpx'``)``.margin(``'10lpx'``)``.backgroundColor(Color.Black)``.borderRadius(5)``.backgroundColor(Color.Orange)``.fontColor(Color.White)``.clickEffect({ level: ClickEffectLevel.LIGHT })``.onClick(() => {``this``.animationCurve = Curve.EaseOut``})``Text(``'EaseInOut动画'``)``.textAlign(TextAlign.Center)``.width(``'200lpx'``)``.height(``'200lpx'``)``.margin(``'10lpx'``)``.backgroundColor(Color.Black)``.borderRadius(5)``.backgroundColor(Color.Orange)``.fontColor(Color.White)``.clickEffect({ level: ClickEffectLevel.LIGHT })``.onClick(() => {``this``.animationCurve = Curve.EaseInOut``})``}.width(``'660lpx'``)``}.width(``'100%'``).justifyContent(FlexAlign.Center)``}``.height(``'100%'``)``.width(``'100%'``)``.backgroundColor(``"#f9feff"``)``}``}` |
| --- | --- |



　　


