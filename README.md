<Button
android:id="@+id/button_record"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="Record" />

<View
android:id="@+id/waveform_view"
android:layout_width="match_parent"
android:layout_height="150dp"
android:background="@android:color/white" />




```java
private MediaRecorder mediaRecorder;
private Visualizer visualizer;
private final int VISUALIZER_HEIGHT = 150;

// ...

private void initMediaRecorder() {
        // 初始化 MediaRecorder
        mediaRecorder = new MediaRecorder();
        mediaRecorder.setAudioSource(MediaRecorder.AudioSource.MIC);
        mediaRecorder.setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP);
        mediaRecorder.setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB);
        mediaRecorder.setOutputFile(getExternalCacheDir().getAbsolutePath() + "/recording.3gp");
        }

private void initVisualizer() {
        // 初始化 Visualizer
        visualizer = new Visualizer(mediaRecorder.getAudioSessionId());
        visualizer.setCaptureSize(Visualizer.getCaptureSizeRange()[1]);
        visualizer.setDataCaptureListener(new Visualizer.OnDataCaptureListener() {
@Override
public void onWaveFormDataCapture(Visualizer visualizer, byte[] waveform, int samplingRate) {
        // 实现绘制水波纹效果的逻辑
        // ...
        }

@Override
public void onFftDataCapture(Visualizer visualizer, byte[] fft, int samplingRate) {
        // ...
        }
        }, Visualizer.getMaxCaptureRate() / 2, true, false);
        }

// ...

```



buttonRecord.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View v) {
// 启动 MediaRecorder
try {
mediaRecorder.prepare();
mediaRecorder.start();
} catch (IOException e) {
e.printStackTrace();
}

        // 启动 Visualizer
        visualizer.setEnabled(true);
    }
});


@Override
protected void onDestroy() {
super.onDestroy();

    // 释放 MediaRecorder
    mediaRecorder.release();

    // 释放 Visualizer
    visualizer.release

