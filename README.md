# XCPullToLoadMoreListView
This is a custom pull-down-to-load-more listview layout project,which is such as QQ chat or wechat chat listview。

XCPullToLoadMoreListView-下拉加载更多ListView控件（仿QQ、微信聊天对话列表控件）

效果图：

![image](https://github.com/jczmdeveloper/XCPullToLoadMoreListView/blob/master/screenshots/01.gif)  

![image](https://github.com/jczmdeveloper/XCPullToLoadMoreListView/blob/master/screenshots/02.gif)  

使用方法示例：



package com.xc.demo;

import android.graphics.Color;
import android.os.Bundle;
import android.os.Handler;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.Gravity;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AbsListView;
import android.widget.BaseAdapter;
import android.widget.ListView;
import android.widget.TextView;

import com.xc.util.DensityUtil;
import com.xc.xcpulltoloadmorelistview.R;
import com.xc.xcpulltoloadmorelistview.XCPullToLoadMoreListView;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    ListView mListView;
    MyAdapter mAdapter;
    XCPullToLoadMoreListView mPTLListView;
    List<String> mList = new ArrayList<>(0);
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        init();

    }

    private void init() {
        for(int i = 191;i <=200; i ++){
            mList.add("Item "+ i);
        }
        mPTLListView = (XCPullToLoadMoreListView) findViewById(R.id.list);

        mPTLListView.setOnRefreshListener(new XCPullToLoadMoreListView.OnRefreshListener() {

            @Override
            public void onPullDownLoadMore() {
                Log.v("czm", "onRefreshing");
                new Handler().postDelayed(new Runnable() {
                    @Override
                    public void run() {

                        List<String> list = new ArrayList<String>();
                        int i = 200 - mList.size() - 10 + 1;
                        int count = 0;
                        while (count < 10) {
                            list.add("Item " + i);
                            i++;
                            count++;
                        }
                        mList.addAll(0, list);
                        mAdapter.notifyDataSetChanged();
                        mPTLListView.onRefreshComplete();
                    }
                }, 1000);
            }
        });

        mListView = mPTLListView.getListView();
        mAdapter = new MyAdapter();
        mListView.setAdapter(mAdapter);
    }
    private class MyAdapter extends BaseAdapter {
        @Override
        public int getCount() {
            return mList.size();
        }

        @Override
        public Object getItem(int position) {
            return mList.get(position);
        }

        @Override
        public long getItemId(int position) {
            return position;
        }

        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            ViewHolder holder = null;
            if (convertView == null) {
                holder = new ViewHolder();
                convertView = new TextView(getApplicationContext());
                convertView.setLayoutParams(new AbsListView.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT
                        , DensityUtil.dip2px(getApplicationContext(), 80)));
                holder.text = (TextView) convertView;
                convertView.setTag(holder);
            } else {
                holder = (ViewHolder) convertView.getTag();
            }
            holder.text.setText(mList.get(position));
            holder.text.setTextColor(Color.BLACK);
            holder.text.setGravity(Gravity.CENTER_VERTICAL);
//            holder.text.setBackgroundColor(Color.WHITE);
            return convertView;
        }

        class ViewHolder{
            TextView text;
        }
    }

}

