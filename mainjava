package com.example.amenoimi.test_2018_7_9_pm_2_35;

import android.Manifest;
import android.annotation.SuppressLint;
import android.annotation.TargetApi;
import android.app.Activity;
import android.app.Dialog;
import android.app.ProgressDialog;
import android.content.ContentUris;
import android.content.Context;
import android.content.Intent;
import android.database.Cursor;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.net.Uri;
import android.os.AsyncTask;
import android.os.Bundle;
import android.os.Environment;
import android.os.Handler;
import android.os.Message;
import android.provider.DocumentsContract;
import android.provider.MediaStore;
import android.support.annotation.Nullable;
import android.support.v4.app.FragmentActivity;
import android.support.v7.app.AppCompatActivity;
import android.util.JsonReader;
import android.util.Log;
import android.view.View;
import android.view.WindowManager;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.LinearLayout;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.ScrollView;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.VideoView;

import com.google.android.gms.auth.api.Auth;
import com.google.android.gms.auth.api.signin.GoogleSignIn;
import com.google.android.gms.auth.api.signin.GoogleSignInAccount;
import com.google.android.gms.auth.api.signin.GoogleSignInClient;
import com.google.android.gms.auth.api.signin.GoogleSignInOptions;
import com.google.android.gms.common.ConnectionResult;
import com.google.android.gms.common.GoogleApiAvailability;
import com.google.android.gms.common.api.ApiException;
import com.google.android.gms.common.api.GoogleApiClient;
import com.google.android.gms.common.api.Scope;
import com.google.android.gms.tasks.Task;
import com.google.api.client.extensions.android.http.AndroidHttp;
import com.google.api.client.googleapis.extensions.android.gms.auth.GoogleAccountCredential;
import com.google.api.client.googleapis.json.GoogleJsonResponseException;
import com.google.api.client.googleapis.media.MediaHttpUploader;
import com.google.api.client.googleapis.media.MediaHttpUploaderProgressListener;
import com.google.api.client.http.HttpTransport;
import com.google.api.client.http.InputStreamContent;
import com.google.api.client.json.Json;
import com.google.api.client.json.JsonFactory;
import com.google.api.client.json.jackson2.JacksonFactory;
import com.google.api.client.util.DateTime;
import com.google.api.client.util.ExponentialBackOff;
import com.google.api.services.youtube.YouTube;
import com.google.api.services.youtube.YouTube.Videos;
import com.google.api.services.youtube.YouTubeScopes;
import com.google.api.services.youtube.model.Channel;
import com.google.api.services.youtube.model.ChannelListResponse;
import com.google.api.services.youtube.model.Video;
import com.google.api.services.youtube.model.VideoSnippet;
import com.google.api.services.youtube.model.VideoStatus;

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.FileInputStream;
import java.io.IOError;
import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Calendar;
import java.util.Date;
import java.util.List;

import pub.devrel.easypermissions.EasyPermissions;

public class MainActivity extends AppCompatActivity
        implements View.OnClickListener {

    private MainActivity myThis = this;
    private static final String TAG = "SignInActivity";
    private static final int RC_SIGN_IN = 9001;
    static final int REQUEST_ACCOUNT_PICKER = 1000;
    static final int REQUEST_AUTHORIZATION = 1001;
    static final int REQUEST_GOOGLE_PLAY_SERVICES = 1002;
    static final int REQUEST_PERMISSION_GET_ACCOUNTS = 1003;
    private GoogleSignInClient mGoogleSignInClient;

    private TextView mStatusTextView;
    private GoogleApiClient mGoogleApiClient;
    private GoogleAccountCredential mCredential;
    private static final String[] SCOPES = {YouTubeScopes.YOUTUBE, YouTubeScopes.YOUTUBE_READONLY , YouTubeScopes.YOUTUBE_UPLOAD};

    private FragmentActivity activity ;
    public Button b1,b2,b3,b4,b5,b6;
    public TextView t1,t2;
    public Spinner sp,sp2;
    public EditText et1,et2,et3;
    public VideoView vv;
    public LinearLayout video_up;
    private ListView listView;
    private ListAdapter listAdapter;
    public ScrollView sv;
    private GoogleApiClient googleApiClient;
    private GoogleApiClient fragment;


    private static String PREF_ACCOUNT_NAME = "accountName";//使用者帳號

    public YouTube mService = null;
    public Videos mVideos=null;

    private ProgressDialog progressDialog;

    public Uri up_uri;//選取項目uri

    private static final String VIDEO_FILE_FORMAT = "video/*";

    public String View_settings="unlisted";//設定公布公開(預設不公開)

    public String Video_title="";//設定影片標題
    public String Video_Descrption="";//設定影片說明
    public  List<String> Video_tags = new ArrayList<String>();//設定影片關鍵字
    public String Video_CategoryId="22";//設定類別

    public String channelId;//頻道ID
    public  String result;//搜索API回傳值
    public String UseName="";//使用者名稱
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);

            //-----------------------UI物件配置-----------------------//
        setTitle("影片上傳");
        sv=(ScrollView)findViewById(R.id.sv);
        t1=(TextView)findViewById(R.id.t1);
        t2=(TextView)findViewById(R.id.t2);
        b1=(Button)findViewById(R.id.b1);
        b2=(Button)findViewById(R.id.b2);
        b3=(Button)findViewById(R.id.b3);
        b4=(Button)findViewById(R.id.b4);
        b5=(Button)findViewById(R.id.b5);
        b6=(Button)findViewById(R.id.b6);
        b1.setOnClickListener(this);
        b2.setOnClickListener(this);
        b3.setOnClickListener(this);
        b4.setOnClickListener(this);
        b5.setOnClickListener(this);
        b6.setOnClickListener(this);
        vv = (VideoView) this.findViewById(R.id.videoView);
        PREF_ACCOUNT_NAME="";

        et1=(EditText)findViewById(R.id.et1);
        et2=(EditText)findViewById(R.id.et2);
        et3=(EditText)findViewById(R.id.et3);
        sp = (Spinner)findViewById(R.id.sp);
        sp2 = (Spinner)findViewById(R.id.sp2);

        video_up=(LinearLayout)findViewById(R.id.video_up_lay);

        ArrayAdapter<CharSequence> setting_List = ArrayAdapter.createFromResource(MainActivity.this,
                R.array.setting,
                android.R.layout.simple_spinner_dropdown_item);
        ArrayAdapter<CharSequence> CategoryId_List = ArrayAdapter.createFromResource(MainActivity.this,
                R.array.CategoryId,
                android.R.layout.simple_spinner_dropdown_item);
        sp.setAdapter(setting_List);
        sp2.setAdapter(CategoryId_List);
        sp2.setSelection(10);
            //-----------------------UI物件配置-----------------------//
        sp.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                if( position==0)View_settings="unlisted";//設定公布公開(預設不公開)
                else if( position==1)View_settings="public";//設定公布公開(預設不公開)
                else if( position==2)View_settings="private";//設定公布公開(預設不公開)
            }
            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });


        sp2.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                //設定類別
               switch (position){
                   case 0:
                       Video_CategoryId="2";
                       break;
                   case 1:
                       Video_CategoryId="23";
                       break;
                   case 2:
                       Video_CategoryId="27";
                       break;
                   case 3:
                       Video_CategoryId="24";
                       break;
                   case 4:
                       Video_CategoryId="1";
                       break;
                   case 5:
                       Video_CategoryId="20";
                       break;
                   case 6:
                       Video_CategoryId="26";
                       break;
                   case 7:
                       Video_CategoryId="10";
                       break;
                   case 8:
                       Video_CategoryId="25";
                       break;
                   case 9:
                       Video_CategoryId="29";
                       break;
                   case 10:
                       Video_CategoryId="22";
                       break;
                   case 11:
                       Video_CategoryId="15";
                       break;
                   case 12:
                       Video_CategoryId="28";
                       break;
                   case 13:
                       Video_CategoryId="17";
                       break;
                   case 14:
                       Video_CategoryId="19";
                       break;
               }



            }

            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });

        //-----------------------Google登入設置-----------------------//
        GoogleSignInOptions gso = new GoogleSignInOptions
                .Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestScopes(
                        new Scope(YouTubeScopes.YOUTUBE),
                        new Scope(YouTubeScopes.YOUTUBE_READONLY),
                        new Scope(YouTubeScopes.YOUTUBE_UPLOAD)
                )
                .requestServerAuthCode(getResources().getString(R.string.web_id))//web id
                .requestProfile()
                .requestEmail()
                .build();

        mGoogleApiClient = new GoogleApiClient.Builder(this)
                .addApi(Auth.CREDENTIALS_API)
                .addApi(Auth.GOOGLE_SIGN_IN_API, gso)
                .build();

        // Build a GoogleSignInClient with the options specified by gso.

        mGoogleSignInClient = GoogleSignIn.getClient(this, gso);

        mCredential = GoogleAccountCredential.usingOAuth2(
                getApplicationContext(),
                Arrays.asList(SCOPES)) //設定想使用的權限
                .setBackOff(new ExponentialBackOff());

        //-----------------------Google登入設置-----------------------//

    }

        //-----------------------讀取使用者個人影片-----------------------//
    Runnable run_loder_lists = new Runnable(){

        @Override
        public void run() {
            loder_lists();
        }


    };
    public  void  loder_lists() {
       mCredential.setSelectedAccountName(PREF_ACCOUNT_NAME);
        getResultsFromApi();


        try {

            HttpClient httpClient_get = new DefaultHttpClient();
            HttpGet get_my_id = new HttpGet("https://www.googleapis.com/youtube/v3/search?part=snippet&q="+UseName+"&type=channel&key="+getResources().getString(R.string.API_key));
            HttpResponse id = null;
            id = httpClient_get.execute(get_my_id);
            HttpEntity res_id = id.getEntity();




                JSONObject jsonObject = new JSONObject( EntityUtils.toString(res_id));

                JSONArray array = jsonObject.getJSONArray("items");

                    JSONObject id_jsonObject = array.getJSONObject(0);
                    JSONObject snippet = id_jsonObject.getJSONObject("snippet");
                    channelId = snippet.getString("channelId");

                    Log.d("TAG", "channelId:" + channelId );
            result ="";
            HttpClient httpClient = new DefaultHttpClient();
            //要改API KEY
            HttpGet get = new HttpGet("https://www.googleapis.com/youtube/v3/search?part=snippet&channelId="+channelId+"&type=video&maxResults=50&key="+getResources().getString(R.string.API_key));
            HttpResponse response = null;
            response = httpClient.execute(get);
            HttpEntity resEntity = response.getEntity();
            jsonObject = new JSONObject( EntityUtils.toString(resEntity));
            array = jsonObject.getJSONArray("items");
            for (int i = 0; i < array.length(); i++) {
                JSONObject video_data_tmp = array.getJSONObject(i);
                String v_id=video_data_tmp.getJSONObject("id").getString("videoId");//影片ID
                String v_T=video_data_tmp.getJSONObject("snippet").getString("title");//影片標題
                String img=video_data_tmp.getJSONObject("snippet").getJSONObject("thumbnails").getJSONObject("default").getString("url");//影片縮圖
                result +=v_id+"/"+v_T+"/"+img+"\n";
            }

            Log.d("OWO",result);

            Message msg = mHandler.obtainMessage();
            msg.what = 1;
            msg.sendToTarget();

        } catch (IOException e) {
            e.printStackTrace();
        } catch(JSONException e) {
            e.printStackTrace();
        }


    }
    @SuppressLint("HandlerLeak")
    Handler mHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            if(msg.what == 1) {
//                t2.setText(result);
                listView = (ListView) findViewById(R.id.Video_list);
                String[] re_l=result.split("\n");
                String[] out_l=new String[re_l.length];
                for(int i=0;i<re_l.length;i++){
                    out_l[i]=re_l[i].split("/")[1];
                }
                listAdapter = new ArrayAdapter<String>(myThis, android.R.layout.simple_expandable_list_item_1,out_l);
                listView.setAdapter(listAdapter);
                listView.setOnItemClickListener(onClickListView);
            }
            super.handleMessage(msg);
        }
    };
    private AdapterView.OnItemClickListener onClickListView = new AdapterView.OnItemClickListener(){
        @Override
        public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
            String[] re_l=result.split("\n");
//            re_l[position].split("/")[0]


        }
    };
    //-----------------------讀取使用者個人影片-----------------------//


    private String resolveName(String name) {
        if (name == null) {
            return name;
        }
        if (!name.startsWith("/")) {
            Class<?> c = Class.class;
            while (c.isArray()) {
                c = c.getComponentType();
            }
            String baseName = c.getName();
            int index = baseName.lastIndexOf('.');
            if (index != -1) {
                name = baseName.substring(0, index).replace('.', '/')
                        +"/"+name;
            }
        } else {
            name = name.substring(1);
        }
        return name;
    }
    public InputStream getResourceAsStream(String name) {
        name = resolveName(name);
        ClassLoader cl = getClassLoader();
        if (cl==null) {
            // A system class.
            return ClassLoader.getSystemResourceAsStream(name);
        }
        return cl.getResourceAsStream(name);
    }
    //-----------------------上傳影片-----------------------//
    Runnable run_up_V = new Runnable() {
        @Override
        public void run() {
            up_V();
        }

        public void up_V() {

            try {
                mCredential.setSelectedAccountName(PREF_ACCOUNT_NAME);
                getResultsFromApi();

                Video videoObjectDefiningMetadata = new Video();
                // Set the video to be publicly visible. This is the default
                // setting. Other supporting settings are "unlisted" and "private."
                //設定公不公開"unlisted(不公開)" "private(私人)"
                VideoStatus status = new VideoStatus();
                status.setPrivacyStatus(View_settings);
                //-------------------------------------------------------//

                DateTime now = new DateTime(System.currentTimeMillis());
                DateTime time=new DateTime(new Date().getTime());
                Log.d("dddd",time.toString());
                Log.d("dddd",now.toString());
                // status.setPublishAt(now);//自動發表時間
                //-------------------------------------------------------//



                //                來自youtube上傳頁面
                //                2  - 汽車和車輛
                //                23  - 喜劇
                //                27  - 教育
                //                24  - 娛樂
                //                1  - 電影和動畫
                //                20  - 遊戲
                //                26  - 操作方法和風格
                //                10  - 音樂
                //                25  - 新聞與政治
                //                29  - 非營利組織和行動主義
                //                22  - 人與博客
                //                15  - 寵物和動物
                //                28  - 科學與技術
                //                17  - 運動
                //                19  - 旅遊與活動
            videoObjectDefiningMetadata.setStatus(status);

                VideoSnippet snippet = new VideoSnippet();
                snippet.setCategoryId(Video_CategoryId);//影片類別
                //標題跟內容
                Calendar cal = Calendar.getInstance();
                if(Video_title=="")Video_title="上傳影片： " + cal.getTime();
                snippet.setTitle(Video_title);
                if(Video_Descrption=="")Video_Descrption= PREF_ACCOUNT_NAME+ "於 " + cal.getTime()+"上傳之影片";
                snippet.setDescription(Video_Descrption);
                //關鍵字
                snippet.setTags(Video_tags);

                videoObjectDefiningMetadata.setSnippet(snippet);
                InputStreamContent mediaContent = new InputStreamContent(VIDEO_FILE_FORMAT, new FileInputStream(getMediaAbsolutePath(myThis,up_uri)));

                // Insert the video. The command sends three arguments. The first
                // specifies which information the API request is setting and which
                // information the API response should return. The second argument
                // is the video resource that contains metadata about the new video.
                // The third argument is the actual video content.
                YouTube.Videos.Insert videoInsert = mService.videos().insert("snippet,statistics,status",
                        videoObjectDefiningMetadata,
                        mediaContent);

                // Set the upload type and add an event listener.
                MediaHttpUploader uploader = videoInsert.getMediaHttpUploader();

                // Indicate whether direct media upload is enabled. A value of
                // "True" indicates that direct media upload is enabled and that
                // the entire media content will be uploaded in a single request.
                // A value of "False," which is the default, indicates that the
                // request will use the resumable media upload protocol, which
                // supports the ability to resume an upload operation after a
                // network interruption or other transmission failure, saving
                // time and bandwidth in the event of network failures.
                uploader.setDirectUploadEnabled(false);

                MediaHttpUploaderProgressListener progressListener = new MediaHttpUploaderProgressListener() {
                    public void progressChanged(MediaHttpUploader uploader) throws IOException {
                        switch (uploader.getUploadState()) {
                            case INITIATION_STARTED:
                                System.out.println("Initiation Started");
                                break;
                            case INITIATION_COMPLETE:
                                System.out.println("Initiation Completed");
                                break;
                            case MEDIA_IN_PROGRESS:
                                System.out.println("Upload in progress");
                                break;
                            case MEDIA_COMPLETE:
                                System.out.println("Upload Completed!");
                                break;
                            case NOT_STARTED:
                                System.out.println("Upload Not Started!");
                                break;
                        }
                    }
                };
                uploader.setProgressListener(progressListener);

                // Call the API and upload the video.
                //使用API上傳影片
                Video returnedVideo = videoInsert.execute();

                // Print data about the newly inserted video from the API response.
                System.out.println("\n================== Returned Video ==================\n");
                System.out.println("  - Id: " + returnedVideo.getId());
                System.out.println("  - Title: " + returnedVideo.getSnippet().getTitle());
                System.out.println("  - Tags: " + returnedVideo.getSnippet().getTags());
                System.out.println("  - Privacy Status: " + returnedVideo.getStatus().getPrivacyStatus());
                System.out.println("  - Video Count: " + returnedVideo.getStatistics().getViewCount());

                progressDialog.dismiss();

            } catch (GoogleJsonResponseException e) {
                System.err.println("GoogleJsonResponseException code: " + e.getDetails().getCode() + " : "
                        + e.getDetails().getMessage());
                e.printStackTrace();
            } catch (IOException e) {
                System.err.println("IOException: " + e.getMessage());
                e.printStackTrace();
            } catch (Throwable t) {
                System.err.println("Throwable: " + t.getMessage());
                t.printStackTrace();
            }


        }
    };

    public void l_V(View v){//選擇影片的函式

        final String mimeType = "video/*";

        final Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
        intent.setType(mimeType);
        Intent picker = new Intent(Intent.ACTION_GET_CONTENT);
        picker.setType(mimeType);
        intent.addCategory(Intent.CATEGORY_OPENABLE);
        Intent destIntent = Intent.createChooser(picker, "選取影片");
        startActivityForResult(destIntent,1000);

    }

    public void signIn(View v) {//登入的函式
        Intent signInIntent = mGoogleSignInClient.getSignInIntent();
        startActivityForResult(signInIntent, RC_SIGN_IN);
    }

    public void signout(View v){//登出的函式
        mGoogleSignInClient.signOut();//登出Google

        //---------------UI跟變數的調整---------------//
        t1.setText("點擊登入");
        b1.setVisibility(View.VISIBLE);
        b2.setVisibility(View.GONE);
        b3.setVisibility(View.GONE);
        b4.setVisibility(View.GONE);
        b5.setVisibility(View.GONE);
        et1.setText("");
        et2.setText("");
        b4.setText("選擇影片");
        video_up.setVisibility(View.GONE);
        sv.setVisibility(View.GONE);
        PREF_ACCOUNT_NAME="";
        up_uri=null;
        //---------------UI跟變數的調整---------------//
    }


    //----------------------------------------使用 startActivityForResult() 之後到的地方----------------------------------------//
    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        // Result returned from launching the Intent from GoogleSignInClient.getSignInIntent(...);
        if (requestCode == RC_SIGN_IN) {
            // The Task returned from this call is always completed, no need to attach
            // a listener.
            Task<GoogleSignInAccount> task = GoogleSignIn.getSignedInAccountFromIntent(data);
            handleSignInResult(task);
        }

        // 有選擇檔案
        if ( resultCode == RESULT_OK )
        {
            // 取得檔案的 Uri
            Log.d("QUQ",data.toString());
            Uri uri = data.getData();
            try {
                if (uri != null) {

                    up_uri = uri;
                    vv.setVisibility(View.VISIBLE);
                    vv.setVideoURI(up_uri);
                    vv.start();
                    TextView t1 = (TextView) findViewById(R.id.t1);
                    b4.setText("上傳影片");
                    b5.setVisibility(View.VISIBLE);
                    video_up.setVisibility(View.VISIBLE);
                    sv.setVisibility(View.VISIBLE);
                }
            }catch (IOError e){

            }
        }


    }
    private void handleSignInResult(Task<GoogleSignInAccount> completedTask) {
        try {
            GoogleSignInAccount account = completedTask.getResult(ApiException.class);

            // Signed in successfully, show authenticated UI.
            updateUI(account);
        } catch (ApiException e) {
            // The ApiException status code indicates the detailed failure reason.
            // Please refer to the GoogleSignInStatusCodes class reference for more information.
            Log.w(TAG, "signInResult:failed code=" + e.getStatusCode());
            updateUI(null);
        }
    }
    //----------------------------------------------按鈕點擊監聽----------------------------------------------//
    @Override
    public void onClick(View v) {
        switch (v.getId())
        {
            case R.id.b1:
                signIn(v);
                break;
            case R.id.b2:
                signout(v);
                break;
            case R.id.b3:
//                loder_lists(v);
                new Thread(  run_loder_lists).start();
                break;
            case R.id.b6:
            case R.id.b4:
                if(up_uri ==null ||b4.getText()=="選擇影片"){
                    l_V(v);
                }
                else {
                    if(et1.getText().toString()!=null) Video_title=et1.getText().toString();//設定影片標題
                    if(et2.getText().toString()!=null)Video_Descrption=et2.getText().toString();//設定影片說明
                    if(et3.getText().toString()!=null){
                        Video_tags.addAll(Arrays.asList(et3.getText().toString().split("，")));
                    }

                    progressDialog = new ProgressDialog(myThis);
                    progressDialog.setTitle("YouTube");
                    progressDialog.setMessage("上傳中");
                    progressDialog.setIndeterminate(false);
                    progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
                    progressDialog.show();
                    new Thread(run_up_V).start();
//                    up_V(v);
                    b4.setText("選擇影片");
                    b5.setVisibility(View.GONE);
                    et1.setText("");
                    et2.setText("");
                    et3.setText("");
                    video_up.setVisibility(View.GONE);
                    sv.setVisibility(View.GONE);
                }

                break;
            case R.id.b5:
                progressDialog = new ProgressDialog(this);
                progressDialog.setTitle("YouTube");
                progressDialog.setMessage("搜索中");
                progressDialog.setIndeterminate(false);
                progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
                progressDialog.show();
                l_V(v);
                progressDialog.dismiss();
                break;
        }
    }
    //----------------------------------------------按鈕點擊監聽----------------------------------------------//


    //登入時UI變動
    private void updateUI(@Nullable GoogleSignInAccount account) {
        if (account != null) {
            t1.setText(getString(R.string.signed_in_fmt, account.getDisplayName()));
            UseName= account.getDisplayName();
            PREF_ACCOUNT_NAME=account.getEmail();
           b1.setVisibility(View.GONE);
           b2.setVisibility(View.VISIBLE);
           b3.setVisibility(View.VISIBLE);
           b4.setVisibility(View.VISIBLE);
            b5.setVisibility(View.GONE);
          //  video_up.setVisibility(View.VISIBLE);

        } else {
            t1.setText(R.string.signed_out);

           b1.setVisibility(View.VISIBLE);
           b2.setVisibility(View.GONE);
           b3.setVisibility(View.GONE);
           b4.setVisibility(View.GONE);
            b5.setVisibility(View.GONE);
            et1.setText("");
            et2.setText("");
            video_up.setVisibility(View.GONE);
            sv.setVisibility(View.GONE);
        }
    }

    private void getResultsFromApi() {
        if (! isGooglePlayServicesAvailable()) {
            acquireGooglePlayServices();
        } else if (mCredential.getSelectedAccountName() == null) {
            chooseAccount();
        } else if (! isDeviceOnline()) {
            t1.setText("No network connection available.");
        } else {
            new MakeRequestTask(mCredential).execute();
        }
    }
    private boolean isGooglePlayServicesAvailable() {
        GoogleApiAvailability apiAvailability =
                GoogleApiAvailability.getInstance();
        final int connectionStatusCode =
                apiAvailability.isGooglePlayServicesAvailable(this);
        return connectionStatusCode == ConnectionResult.SUCCESS;
    }
    private void acquireGooglePlayServices() {
        GoogleApiAvailability apiAvailability =
                GoogleApiAvailability.getInstance();
        final int connectionStatusCode =
                apiAvailability.isGooglePlayServicesAvailable(this);
        if (apiAvailability.isUserResolvableError(connectionStatusCode)) {
            showGooglePlayServicesAvailabilityErrorDialog(connectionStatusCode);
        }
    }
    void showGooglePlayServicesAvailabilityErrorDialog(
            final int connectionStatusCode) {
        GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
        Dialog dialog = apiAvailability.getErrorDialog(
                MainActivity.this,
                connectionStatusCode,
                REQUEST_GOOGLE_PLAY_SERVICES);
        dialog.show();
    }
    private void chooseAccount() {
        if (EasyPermissions.hasPermissions(
                this, Manifest.permission.GET_ACCOUNTS)) {
            String accountName = getPreferences(Context.MODE_PRIVATE)
                    .getString(PREF_ACCOUNT_NAME, null);
            if (accountName != null) {
                mCredential.setSelectedAccountName(accountName);
                getResultsFromApi();
            } else {
                // Start a dialog from which the user can choose an account
                startActivityForResult(
                        mCredential.newChooseAccountIntent(),
                        REQUEST_ACCOUNT_PICKER);
            }
        } else {
            // Request the GET_ACCOUNTS permission via a user dialog
            EasyPermissions.requestPermissions(
                    this,
                    "This app needs to access your Google account (via Contacts).",
                    REQUEST_PERMISSION_GET_ACCOUNTS,
                    Manifest.permission.GET_ACCOUNTS);
        }
    }
    private boolean isDeviceOnline() {
        ConnectivityManager connMgr =
                (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo networkInfo = connMgr.getActiveNetworkInfo();
        return (networkInfo != null && networkInfo.isConnected());

    }
    private class MakeRequestTask extends AsyncTask<Void, Void, List<String>> {

        private Exception mLastError = null;

        MakeRequestTask(GoogleAccountCredential credential) {
            HttpTransport transport = AndroidHttp.newCompatibleTransport();
            JsonFactory jsonFactory = JacksonFactory.getDefaultInstance();
            mService = new YouTube.Builder(
                    transport, jsonFactory, credential)
                    .setApplicationName("YouTube Data API Android Quickstart")
                    .build();
            mVideos=mService.videos();

        }
        @Override
        protected List<String> doInBackground(Void... params) {
            try {
                return getDataFromApi();
            } catch (Exception e) {
                mLastError = e;
                cancel(true);
                return null;
            }
        }
        private List<String> getDataFromApi() throws IOException {
            // Get a list of up to 10 files.
            List<String> channelInfo = new ArrayList<String>();
            ChannelListResponse result = mService.channels().list("snippet,contentDetails,statistics")
                    .setForUsername("GoogleDevelopers")
                    .execute();
            List<Channel> channels = result.getItems();
            if (channels != null) {
                Channel channel = channels.get(0);
                channelInfo.add("This channel's ID is " + channel.getId() + ". " +
                        "Its title is '" + channel.getSnippet().getTitle() + ", " +
                        "and it has " + channel.getStatistics().getViewCount() + " views.");
            }
            return channelInfo;
        }
}


//---------------------------------------------------uri轉檔案實體位置---------------------------------------------------//
    @TargetApi(19)
    public static String getMediaAbsolutePath(Activity context, Uri imageUri) {
        if (context == null || imageUri == null)
            return null;
        if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.KITKAT && DocumentsContract.isDocumentUri(context, imageUri)) {
            if (isExternalStorageDocument(imageUri)) {
                String docId = DocumentsContract.getDocumentId(imageUri);
                String[] split = docId.split(":");
                String type = split[0];
                if ("primary".equalsIgnoreCase(type)) {
                    return Environment.getExternalStorageDirectory() + "/" + split[1];
                }
            } else if (isDownloadsDocument(imageUri)) {
                String id = DocumentsContract.getDocumentId(imageUri);
                Uri contentUri = ContentUris.withAppendedId(Uri.parse("content://downloads/public_downloads"), Long.valueOf(id));
                return getDataColumn(context, contentUri, null, null);
            } else if (isMediaDocument(imageUri)) {
                String docId = DocumentsContract.getDocumentId(imageUri);
                String[] split = docId.split(":");
                String type = split[0];
                Uri contentUri = null;
                if ("image".equals(type)) {
                    contentUri = MediaStore.Images.Media.EXTERNAL_CONTENT_URI;
                } else if ("video".equals(type)) {
                    contentUri = MediaStore.Video.Media.EXTERNAL_CONTENT_URI;
                } else if ("audio".equals(type)) {
                    contentUri = MediaStore.Audio.Media.EXTERNAL_CONTENT_URI;
                }
                String selection = MediaStore.Images.Media._ID + "=?";
                String[] selectionArgs = new String[] { split[1] };
                return getDataColumn(context, contentUri, selection, selectionArgs);
            }
        } // MediaStore (and general)
        else if ("content".equalsIgnoreCase(imageUri.getScheme())) {
            // Return the remote address
            if (isGooglePhotosUri(imageUri))
                return imageUri.getLastPathSegment();
            return getDataColumn(context, imageUri, null, null);
        }
        // File
        else if ("file".equalsIgnoreCase(imageUri.getScheme())) {
            return imageUri.getPath();
        }
        return null;
    }

    public static String getDataColumn(Context context, Uri uri, String selection, String[] selectionArgs) {
        Cursor cursor = null;
        String column = MediaStore.Images.Media.DATA;
        String[] projection = { column };
        try {
            cursor = context.getContentResolver().query(uri, projection, selection, selectionArgs, null);
            if (cursor != null && cursor.moveToFirst()) {
                int index = cursor.getColumnIndexOrThrow(column);
                return cursor.getString(index);
            }
        } finally {
            if (cursor != null)
                cursor.close();
        }
        return null;
    }

    /**
     * @param uri The Uri to check.
     * @return Whether the Uri authority is ExternalStorageProvider.
     */
    public static boolean isExternalStorageDocument(Uri uri) {
        return "com.android.externalstorage.documents".equals(uri.getAuthority());
    }

    /**
     * @param uri The Uri to check.
     * @return Whether the Uri authority is DownloadsProvider.
     */
    public static boolean isDownloadsDocument(Uri uri) {
        return "com.android.providers.downloads.documents".equals(uri.getAuthority());
    }

    /**
     * @param uri The Uri to check.
     * @return Whether the Uri authority is MediaProvider.
     */
    public static boolean isMediaDocument(Uri uri) {
        return "com.android.providers.media.documents".equals(uri.getAuthority());
    }

    /**
     * @param uri The Uri to check.
     * @return Whether the Uri authority is Google Photos.
     */
    public static boolean isGooglePhotosUri(Uri uri) {
        return "com.google.android.apps.photos.content".equals(uri.getAuthority());
    }

    //---------------------------------------------------uri轉檔案實體位置---------------------------------------------------//
}
