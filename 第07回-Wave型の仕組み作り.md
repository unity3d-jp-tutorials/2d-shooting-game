

![](https://unity3d.com-jp-learn-tutorials.s3.amazonaws.com/2d-shooting-game/images/game/07/wave.png)



前章では、背景を作りスクロールさせたことにより、よりゲームらしくなってきました。この章では、さらにゲームの中身を作っていきます。今回のチュートリアルはWave型のシューティングゲームです。

7.1　Waveの作成
--------------------------------------

空のGameObjectを作成し、名前を**Wave**としてください。Waveの**位置は（0,0,0）**とします。
次に３つのEnemyを子要素として図7.1のように並べます。



![](https://unity3d.com-jp-learn-tutorials.s3.amazonaws.com/2d-shooting-game/images/game/07/create_wave.png)
<br/>図7.1:



最後にプレハブにして、ゲームオブジェクトの方は削除しましょう。

7.2　Waveを呼び出す**Emitter (エミッター)**を作成する
----------------------------------------------------------------------------

Waveプレハブをスクリプトから作成します。
まず、**Emitter.cs**をScriptsフォルダの中に作成します。



Emitter.cs

```csharp
using UnityEngine;
using System.Collections;

public class Emitter : MonoBehaviour
{
    // Waveプレハブを格納する
    public GameObject[] waves;

    // 現在のWave
    private int currentWave;

    IEnumerator Start ()
    {

        // Waveが存在しなければコルーチンを終了する
        if (waves.Length == 0) {
            yield break;
        }

        while (true) {

            // Waveを作成する
            GameObject wave = (GameObject)Instantiate (waves [currentWave], transform.position, Quaternion.identity);

            // WaveをEmitterの子要素にする
            wave.transform.parent = transform;

            // Waveの子要素のEnemyが全て削除されるまで待機する
            while (wave.transform.childCount != 0) {
                yield return new WaitForEndOfFrame ();
            }

            // Waveの削除
            Destroy (wave);

            // 格納されているWaveを全て実行したらcurrentWaveを0にする（最初から -> ループ）
            if (waves.Length <= ++currentWave) {
                currentWave = 0;
            }

        }
    }
}
```



次に空のゲームオブジェクトを作成し、名前を**Emitter**としてください。ゲームオブジェクトの**位置はX
0 Y 3.5 Z 0**とします。
EmitterゲームオブジェクトにEmitter.csをアタッチし、Waveプレハブを図7.2のように格納します。



![](https://unity3d.com-jp-learn-tutorials.s3.amazonaws.com/2d-shooting-game/images/game/07/emitter_inspector.png)
<br/>図7.2:



この状態でゲームを再生してみてください。
敵を倒したり通り過ぎて行くと再度Waveが登場します。



![弾が多く敵を倒せない場合はWaveプレハブにあるEnemyのCanShotのチェックを外してください](https://unity3d.com-jp-learn-tutorials.s3.amazonaws.com/2d-shooting-game/images/game/07/play.png)
<br/>図7.3:
弾が多く敵を倒せない場合はWaveプレハブにあるEnemyのCanShotのチェックを外してください



### 第07回終わり

今回はここで終了です。つまずいてしまった方はプロジェクトファイルをダウンロードして新たな気持ちで次の回へ進みましょう。

[今回のプロジェクトファイルをダウンロード](https://unity3d.com-jp-learn-tutorials.s3.amazonaws.com/2d-shooting-game/project/game_07_ShootingGame.zip)
