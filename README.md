# OKListView

![MacDown logo](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1492965055221&di=7a2ddb717d591f43b565befca344995f&imgtype=0&src=http%3A%2F%2Fimage.tianjimedia.com%2FuploadImages%2F2014%2F302%2F57%2FPGA7S9OB8F54.jpg)

## 解决问题
* 数据下拉刷新
* 数据上拉加载
* 动画效果
* 加载优化

## 基本思想
* ListView头部与尾部加载
* 属性动画确定padding大小
* onTouch事件处理

## 使用方法

### 初始化ListViw，并设置是否可以加载更多或下拉刷新功能。

	rListView = (OKListView) findViewById(R.id.refreshlistview);
        // 是否允许加载更多
        // rListView.setCanLoadMore(false);
        // 是否允许下拉刷新
        // rListView.setCanRefresh(false);
        textList = new ArrayList<>();
        for (int i = 0; i < 40; i++) {
            textList.add("这是一条ListView的数据" + i);
        }
        adapter = new MyAdapter();
        rListView.setAdapter(adapter);
        rListView.setOnRefreshListener(this);

### 下拉刷新跟加载更多实现接口的回调

	@Override
    public void onDownPullRefresh() {
        new AsyncTask<Void, Void, Void>() {

            @Override
            protected Void doInBackground(Void...params) {
                SystemClock.sleep(3000);
                for (int i = 0; i < 2; i++) {
                    textList.add(0, "这是下拉刷新出来的数据" + i);
                }
                return null;
            }

            @Override
            protected void onPostExecute(Void result) {
                adapter.notifyDataSetChanged();
                rListView.hideHeaderView(true);
            }
        }.execute(new Void[] {});
    }

    @Override
    public void onLoadingMore() {
        new AsyncTask<Void, Void, Void>() {

            @Override
            protected Void doInBackground(Void...params) {
                SystemClock.sleep(3000);

                textList.add("这是加载更多出来的数据1");
                textList.add("这是加载更多出来的数据2");
                textList.add("这是加载更多出来的数据3");
                return null;
            }

            @Override
            protected void onPostExecute(Void result) {
                adapter.notifyDataSetChanged();

                // 控制脚布局隐藏
                rListView.hideFooterView();
            }
        }.execute(new Void[] {});
    }