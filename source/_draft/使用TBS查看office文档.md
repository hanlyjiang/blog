---
title: Android使用TBS查看office文档
date: 2017-11-05 22:15:40
tags:
- Android
- TBS
---
```java
/**
 * 文件浏览视图
 *
 * @author hanlyjiang
 */
public class TBSFileViewActivity extends AppCompatActivity implements TbsReaderView.ReaderCallback {

    public static final String FILE_PATH = "filePath";
    protected FrameLayout flReadContent;
    private TbsReaderView mTbsReaderView;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        String filePath = handleIntent();
        if (TextUtils.isEmpty(filePath) || !new File(filePath).isFile()) {
            Toast.makeText(this, "文件不存在", Toast.LENGTH_SHORT).show();
            finish();
        }

        getSupportActionBar().setTitle(filePath);

        mTbsReaderView = new TbsReaderView(this, this);
        FrameLayout.LayoutParams layoutParams = new FrameLayout.LayoutParams(
                FrameLayout.LayoutParams.MATCH_PARENT,
                FrameLayout.LayoutParams.MATCH_PARENT
        );
        mTbsReaderView.setLayoutParams(layoutParams);
        setContentView(mTbsReaderView, layoutParams);

        displayFile(filePath);
    }

    private String handleIntent() {
        if (getIntent() != null) {
            return getIntent().getStringExtra(FILE_PATH);
        }
        return null;
    }


    @Override
    public void onCallBackAction(Integer integer, Object long1, Object long2) {

    }


    private void displayFile(String fileAbsPath) {
        Bundle bundle = new Bundle();
        bundle.putString("filePath", fileAbsPath);
        bundle.putString("tempPath", Environment.getExternalStorageDirectory().getPath());
        boolean result = mTbsReaderView.preOpen(parseFormat(fileAbsPath), false);
        if (result) {
            mTbsReaderView.openFile(bundle);
        }else{
            Toast.makeText(this,"文件格式不支持", Toast.LENGTH_SHORT).show();
        }
    }


    private String parseFormat(String fileName) {
        return fileName.substring(fileName.lastIndexOf(".") + 1);
    }
    /**
     * 获取文件后缀名
     *
     * @param fileAbsPath
     * @return
     */
    private String getFileExt(String fileAbsPath) {
        if (!TextUtils.isEmpty(fileAbsPath)) {
            String[] split = fileAbsPath.split("\\.");
            return "." + split[split.length - 1];
        }
        return "unkownFormat";
    }

    @Override
    protected void onStop() {
        super.onStop();
        mTbsReaderView.onStop();
    }
}

```
