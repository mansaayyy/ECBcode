# ECBcode

EMAIL CODE

package com.example.manasi.emailapp;

        import android.support.v7.app.AppCompatActivity;
        import android.os.Bundle;

        import android.net.Uri;
        import android.os.Bundle;
        import android.app.Activity;
        import android.content.Intent;
        import android.util.Log;
        import android.view.Menu;
        import android.view.View;
        import android.widget.Button;
        import android.widget.Toast;

public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button startBtn = (Button) findViewById(R.id.sendEmail);
        startBtn.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {
                sendEmail();
            }
        });
    }

    protected void sendEmail() {
        Log.i("Send email", "");
        String[] TO = {""};
        String[] CC = {""};
        Intent emailIntent = new Intent(Intent.ACTION_SEND);

        emailIntent.setData(Uri.parse("mailto:"));
        emailIntent.setType("text/plain");
        emailIntent.putExtra(Intent.EXTRA_EMAIL, TO);
        emailIntent.putExtra(Intent.EXTRA_CC, CC);
        emailIntent.putExtra(Intent.EXTRA_SUBJECT, "Your subject");
        emailIntent.putExtra(Intent.EXTRA_TEXT, "Email message  goes here");

        try {
            startActivity(Intent.createChooser(emailIntent, "Send mail..."));
            finish();
        } catch (android.content.ActivityNotFoundException ex) {
            Toast.makeText(MainActivity.this, "There is no email client installed.", Toast.LENGTH_SHORT).show();
        }
    }
}





CAMERA CODE

package com.example.manasi.cameraapp;

import android.provider.MediaStore;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

import android.app.Activity;
import android.content.Intent;
import android.graphics.Bitmap;
import android.os.Bundle;
import android.view.Menu;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;

public class MainActivity extends Activity {
    private static final int CAMERA_REQUEST=1888;
    ImageView imageView;

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        imageView=(ImageView)this.findViewById(R.id.imageView1);

        Button photoButton=(Button)this.findViewById(R.id.button1);

        photoButton.setOnClickListener(new View.OnClickListener(){
        @Override
        public void onClick(View v){
            Intent cameraIntent=new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
            startActivityForResult(cameraIntent,CAMERA_REQUEST);
        }
    });
}

    protected void onActivityResult(int requestCode,int resultCode, Intent data) {
        if(requestCode == CAMERA_REQUEST){
            Bitmap photo =(Bitmap)data.getExtras().get("data");
            imageView.setImageBitmap(photo);
        }
    }
}






BATTERY CODE

package com.example.manasi.battery2;

        import android.content.BroadcastReceiver;
        import android.content.Context;
        import android.content.Intent;
        import android.content.IntentFilter;
        import android.os.BatteryManager;
        import android.support.v7.app.AppCompatActivity;
        import android.os.Bundle;
        import android.widget.TextView;
        import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    private TextView batteryTxt;
    private TextView charge;
    private TextView Acusb;
    private BroadcastReceiver mBatInfoReceiver = new BroadcastReceiver(){
        @Override
        public void onReceive(Context ctxt, Intent intent) {
            int level = intent.getIntExtra(BatteryManager.EXTRA_LEVEL, 0);
            batteryTxt.setText(String.valueOf(level) + "%");
            int status = intent.getIntExtra(BatteryManager.EXTRA_STATUS, -1);



            boolean isCharging = status == BatteryManager.BATTERY_STATUS_CHARGING ||
                    status == BatteryManager.BATTERY_STATUS_FULL;
            if (isCharging){
                charge.setText("The device is charging");

            }else{
                charge.setText("The device is not charging");

            }
            int status1 = intent.getIntExtra(BatteryManager.EXTRA_STATUS, -1);

            boolean plug = status1 == BatteryManager.BATTERY_PLUGGED_AC ||
                    status1 == BatteryManager.BATTERY_PLUGGED_USB;

            if (plug){
                Acusb.setText("BATTERY PLUGGED AC");

            }else{
                Acusb.setText("BATTERY PLUGGED USB");

            }
        }
    };

    @Override
    public void onCreate(Bundle b) {
        super.onCreate(b);
        setContentView(R.layout.activity_main);
        batteryTxt = (TextView) this.findViewById(R.id.batteryTxt);
        charge = (TextView) this.findViewById(R.id.charging);
        Acusb = (TextView) this.findViewById(R.id.acusb);

        this.registerReceiver(this.mBatInfoReceiver, new IntentFilter(Intent.ACTION_BATTERY_CHANGED));
    }

}

