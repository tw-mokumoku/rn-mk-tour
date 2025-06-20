<h1 align="center">RN-MK-Tour</h1>

<p align="center">
  A flexible <strong>tourguide</strong> for your react native app!

## Installation

```bash
npm install rn-mk-tour
```

```bash
npm install react-native-svg
react-native link react-native-svg
```

If you are using Expo:

```bash
npx expo install react-native-svg
```

## Usage
```ts
import {
  RnmkTourProvider,
  useRnmkTourController
} from 'rn-mk-tour';

// --- カスタムツアーパネルコンポーネント ---
const WelcomeTourPanel = ({ onNext }) => (
  <View style={styles.panelContainer}>
    <Text style={styles.panelTitle}>ようこそ！</Text>
    <Text style={styles.panelDescription}>アプリの簡単な使い方を説明します。</Text>
    <Button title="次へ" onPress={onNext} />
  </View>
);

const FeatureTourPanel = ({ onBack, onNext }) => (
  <View style={styles.panelContainer}>
    <Text style={styles.panelTitle}>メイン機能はこちら</Text>
    <Text style={styles.panelDescription}>このボタンで重要な機能が使えます。</Text>
    <View style={styles.panelButtonGroup}>
      <Button title="戻る" onPress={onBack} />
      <Button title="ツアー完了" onPress={onNext} />
    </View>
  </View>
);

function App(){
  return (
    <RnmkTourProvider>
      <AppContent />
    </RnmkTourProvider>
  );
}

function AppContent(){
  const {
    canStart:defaultCanStart, // a boolean indicate if you can start tour guide
    start:defaultStart, // a function to start the tourguide
    stop:defaultStop, // a function to stopping it
    next: defaultNext,
    TourZone:DefaultTourZone, // 
  } = useRnmkTourController('default');

  React.useEffect(()=>{
    if(defaultCanStart){
      defaultStart();
    }
  },[defaultCanStart])

  return (
    <View style={styles.container}>
      <Text style={styles.header}>RN-mk-Tour デモアプリ</Text>

      <DefaultTourZone
        step={0}
        content={<WelcomeTourPanel onNext={defaultNext} />}
      >
        <Text style={styles.highlightText1}>これは最初のターゲットです。</Text>
      </DefaultTourZone>

      <View style={styles.spacer} />

      <DefaultTourZone
        step={1}
        content={<FeatureTourPanel onBack={defaultNext} onNext={defaultStop} />}
      >
        <Button title="メイン機能ボタン" onPress={() => console.log('メイン機能')} />
      </DefaultTourZone>

      <View style={styles.spacer} />

      <Button title="手動でツアーを停止" onPress={defaultStop} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
  },
  header: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 50,
  },
  highlightText1: {
    fontSize: 20,
    color: 'blue',
    fontWeight: 'bold',
    padding: 10,
    borderWidth: 1,
    borderColor: 'blue',
    borderRadius: 5,
  },
  spacer: {
    height: 50, // レイアウト調整用のスペース
  },
  // パネル関連のスタイル
  panelContainer: {
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 8,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5,
  },
  panelTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 5,
  },
  panelDescription: {
    fontSize: 14,
    textAlign: 'center',
    marginBottom: 15,
  },
  panelButtonGroup: {
    flexDirection: 'row',
    gap: 10, // RN 0.71+ で利用可能
  },
});
```

## Interfaces & Types & Hooks
### Interface
<details>
  <summary>RnmkTourProviderProps</summary>
  <p>

```ts
interface RnmkTourProviderProps{
  androidStatusBarVisible?: boolean
  children: React.ReactNode
  preventOutsideInteraction?:boolean
}
```
  </p>
</details>

| Name                      | Type      | Default value | Description                                                                   |
| :------------------------ | :-------- | :------------ | :---------------------------------------------------------------------------- |
| `androidStatusBarVisible` | `boolean` | `true`        | Controls the visibility of the status bar on Android.                         |
| `children`                | `ReactNode` | `null`        | The JSX elements that will be the target of the tour.                         |
| `preventOutsideInteraction` | `boolean` | `true`        | Whether to disable taps outside of the highlighted element and the tour panel. |

<details>
  <summary>TourZoneProps</summary>
  <p>
    
```ts
interface TourZoneProps{
  zone: number // A positive number indicating the order of the step in the entire walkthrough.
  shape?: Shape // which shape
  maskOffset?: number // offset around zone
  borderRadius?: number // round corner when rectangle
  keepTooltipPosition?: boolean
  children: React.ReactNode
  content: React.ReactNode
  backdropColor?: string
  borderRadius?: number
  animationDuration?: number
  children: React.ReactNode
}
```
  </p>
</details>

| Name                | Type        | Default value           | Description                                                                                             |
| :------------------ | :---------- | :---------------------- | :------------------------------------------------------------------------------------------------------ |
| `zone`              | `number`    | -                       | A positive number indicating the order of the step in the entire walkthrough.                           |
| `shape`             | `Shape`     | `'rectangle'`           | The shape of the highlight mask (`'circle'` or `'rectangle'`).                                          |
| `maskOffset`        | `number`    | `0`                     | An offset around the highlighted zone, increasing the mask area.                                        |
| `borderRadius`      | `number`    | `0`                     | The corner radius for a `rectangle` shape.                                                              |
| `keepTooltipPosition` | `boolean`   | `false`                 | If `true`, the tooltip position remains fixed relative to the screen, not the highlighted element.      |
| `children`          | `ReactNode` | `null`                  | The React Native component that this tour step will highlight.                                          |
| `content`           | `ReactNode` | `null`                  | The custom JSX content to be displayed within the tour panel for this step.                             |
| `backdropColor`     | `string`    | `'rgba(0,0,0,0.7)'`     | The color of the overlay backdrop behind the highlight.                                                 |
| `animationDuration` | `number`    | `300` (ms)              | The duration of highlight and panel animations in milliseconds.                                         |

### Type
<details>
  <summary>Shape</summary>
  <p>
    
```ts
type Shape = 'circle' | 'rectangle'
```
  </p>
</details>

| Name    | Type     | Default value | Description                                          |
| :------ | :------- | :------------ | :--------------------------------------------------- |
| `Shape` | `string` | -             | Defines the geometric shape used for the highlight mask. |

### Hook
<details>
  <summary>useRnmkTourController</summary>
  <p>
    
```ts
const {
  canStart,         // boolean
  start,            // () => void
  back,             // () => void
  next,             // () => void
  restart,          // () => void
  skip,             // () => void
  stop,             // () => void
  getCurrentStep,   // () => number
  TourZone,         // React.ComponentType<TourZoneProps & { tourKey: string; step: number; content: React.ReactNode; }>
} = useRnmkTourController(tourKey: string);
```

<details>
  <summary>useRnmkTourController Detail</summary>
  <p>

### Parameters

* **`tourKey`**: `string`
    * A **unique identifier** for the tour you want to control. This is super handy if you've got multiple independent tours in your app.

### Return Values

* **`canStart`**: `boolean`
    * This boolean tells you if the tour is **ready to kick off**. It turns `true` once all the tour targets for your specified `tourKey` are properly registered.
* **`start`**: `() => void`
    * Call this function to **begin the tour** from its very first step for the `tourKey` you provided.
* **`back`**: `() => void`
    * Use this to **go back to the previous step** in the tour.
* **`next`**: `() => void`
    * This function lets you **move forward to the next step** in the tour.
* **`restart`**: `() => void`
    * Calling `restart()` will **start the tour over** from the beginning (step 0) for your `tourKey`.
* **`skip`**: `() => void`
    * Use `skip()` to **exit the current tour immediately**.
* **`stop`**: `() => void`
    * This function will **completely halt the tour**. Once stopped, the tour won't pop up again unless you explicitly start it (e.g., by calling `start()` or `restart()`).
* **`getCurrentStep`**: `() => number`
    * This function **returns the current step number** of the tour that's active.
* **`TourZone`**: `React.ComponentType<TourZoneProps & { tourKey: string; step: number; content: React.ReactNode; }>`
    * This is the **component that actually shows your tour's visuals**, like the overlay backdrop and the content panel. You'll want to place it high up in your app's component tree (usually as a child of `RnmkTourProvider`) so it can correctly overlay other UI elements. It accepts `TourZoneProps` for customization (like `contentType` or `overlayStyle`), and also needs `tourKey`, `step`, and `content` when you define your tour steps.
  </p>
</details>

  </p>
</details>
