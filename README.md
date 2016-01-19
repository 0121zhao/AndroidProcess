# �ж�App�Ƿ���ǰ̨�ķ���

�����󵯳���Ӧ��Notification�����û����Notification��ʱ��ҲҪ������Ӧ�Ĵ������ǵ�App�п�������ǰ̨���У�Ҳ�п����ں�̨���У���ʱ�������ڴ������¼���ʱ�򲻵ò������жϣ�App����������ǰ̨�����ں�̨�أ�������ò�ƺܼ򵥣�����Android SDKӦ�û��ṩ��Ӧ��API�������ߵ��ã�����ǲ�û�У����ǲ��ò�ͨ�������ֶ�����ȡApp��ǰ��״̬��


����һ��ͨ��RunningTask
-----
**ԭ��**
��һ��App����ǰ̨��ʱ�򣬻ᴦ��RunningTask�����ջ��ջ�����������ǿ���ȡ��RunningTask��ջ����������̣����������ǵ���Ҫ�жϵ�App�İ����Ƿ���ͬ�����ﵽЧ��
``` java 
    /**
     * ����1��ͨ��getRunningTasks�ж�App�Ƿ�λ��ǰ̨���˷�����5.0����ʧЧ
     *
     * @param context     �����Ĳ���
     * @param packageName ��Ҫ����Ƿ�λ��ջ����App�İ���
     * @return
     */
    public static boolean getRunningTask(Context context, String packageName) {
        ActivityManager am = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        ComponentName cn = am.getRunningTasks(1).get(0).topActivity;
        return !TextUtils.isEmpty(packageName) && packageName.equals(context.getPackageName());
    }
```

**ȱ��**  
getRunningTasks���������5.0 Lollipop֮��Ͳ��ڶԵ�����App�����ˣ���Ϊ������й©�û�����Ϣ��Ϊ�������ݣ��������ֻ����һС���ֵ����߱����task������һЩ��̫���е�task��

**����**
��������С�׵Ļ����ϴ�ӡ�Ľ����Android�İ汾��Android4.4.4�ģ������ǿ����õ�ȫ�����������е�Ӧ�õ���Ϣ�ġ����а���Ϊcom.whee.wheetalklollipop�ľ���������Ҫ�ж��Ƿ���ǰ̨��App�������λ�ڵ�һ����˵���Ǵ���ǰ̨��
![enter image description here](https://raw.githubusercontent.com/wenmingvs/NotificationUtil/develop_notify/%E5%B0%8F%E7%B1%B3%E6%89%93%E5%8D%B0.PNG)

������Android5.0�ϴ�ӡ�Ľ����������Ȼ���˺ܶ���������΢�����������ţ�QQ�����App�����Ǵ�ӡ������ȴ��ȫ�������ˣ�ֻ�������App����Ϣ��һЩϵͳ���̵���Ϣ����˵��Android5.0��ȷ��������ôһ�������ˣ�ֻ����һС���ֵ����߱����task������һЩ��̫���е�task��
![enter image description here](https://raw.githubusercontent.com/wenmingvs/NotificationUtil/develop_notify/2.PNG)

�鿴�����˵�������׻���������App�İ��������᷵��ʲô�����ǣ���������ôһ�仰����possibly some other tasks
     * such as home that are known to not be sensitive.��˵���п��ܲ����᷵��һЩ�����е���Ϣ�������ʹ���ֻ�������App�İ������������ô��ʹ�˻ص���̨������Ȼ�ᱻ�ж�Ϊǰ̨���˷�����Ȼ����ȱ�ݵ�
��
``` java
@deprecated As of {@link android.os.Build.VERSION_CODES#LOLLIPOP}, this method
     * is no longer available to third party
     * applications: the introduction of document-centric recents means
     * it can leak person information to the caller.  For backwards compatibility,
     * it will still retu rn a small subset of its data: at least the caller's
     * own tasks, and possibly some other tasks
     * such as home that are known to not be sensitive.
```


��������ͨ��RunningProcess
-----
**ԭ��**
ͨ��runningProcess��ȡ��һ����ǰ�������еĽ��̵�List�����Ǳ������List�е�ÿһ�����̣��ж�������̵�һ��importance �����Ƿ���ǰ̨���̣����Ұ����Ƿ��������жϵ�APp�İ���һ����������������������ϣ���ô���App�ʹ���ǰ̨
``` java
   /**
     * ����2��ͨ��getRunningAppProcesses��IMPORTANCE_FOREGROUND�����ж��Ƿ�λ��ǰ̨����service��Ҫ��פ��̨ʱ�򣬴˷���ʧЧ,
     * ��С�� Note�ϴ˷�����Ч����Nexus������
     *
     * @param context     �����Ĳ���
     * @param packageName ��Ҫ����Ƿ�λ��ջ����App�İ���
     * @return
     */
    public static boolean getRunningAppProcesses(Context context, String packageName) {
        ActivityManager activityManager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningAppProcessInfo> appProcesses = activityManager.getRunningAppProcesses();
        if (appProcesses == null) {
            return false;
        }
        for (ActivityManager.RunningAppProcessInfo appProcess : appProcesses) {
            if (appProcess.importance == ActivityManager.RunningAppProcessInfo.IMPORTANCE_FOREGROUND && appProcess.processName.equals(packageName)) {
                return true;
            }
        }
        return false;
    }
```

**ȱ��**��
���������͵�App�У�������Ҫ��פ��̨������ϵĻ�ȡ����������Ϣ�������Ҫ���ǰ�Service���ó�START_STICKY��kill ��ᱻ�������ȴ�5�����ң�����֤Service��פ��̨�����Service������������ԣ����App�Ľ��̾ͻᱻ�ж���ǰ̨�������ϵı��־���appProcess.importance��ֵ��Զ�� ActivityManager.RunningAppProcessInfo.IMPORTANCE_FOREGROUND����������Զ�޷��жϳ������ĸ���ǰ̨�ˡ�




��������ͨ��ActivityLifecycleCallbacks
------
**ԭ��**
AndroidSDK14��Application����������ActivityLifecycleCallbacks�����ǿ���ͨ�����Callback�õ�App����Activity���������ڻص���
``` java
 public interface ActivityLifecycleCallbacks {
        void onActivityCreated(Activity activity, Bundle savedInstanceState);
        void onActivityStarted(Activity activity);
        void onActivityResumed(Activity activity);
        void onActivityPaused(Activity activity);
        void onActivityStopped(Activity activity);
        void onActivitySaveInstanceState(Activity activity, Bundle outState);
        void onActivityDestroyed(Activity activity);
    }
```
֪����Щ��Ϣ�����ǾͿ����ø��ٷ��İ취��������⣬��Ȼ�������÷��������Activity�������ڵ����ԣ�����ֻ��Ҫ��Application��onCreat������ȥע�������ӿڣ�Ȼ����Activity�ص���������״̬���ɡ��������£�

``` java
public class MyApplication extends Application {
    private int appCount = 0;

    @Override
    public void onCreate() {
        super.onCreate();
        registerActivityLifecycleCallbacks(new ActivityLifecycleCallbacks() {
            @Override
            public void onActivityCreated(Activity activity, Bundle savedInstanceState) {
                Log.d("wenming", "onActivityCreated");
            }

            @Override
            public void onActivityStarted(Activity activity) {
                Log.d("wenming", "onActivityStarted");
                appCount++;
            }

            @Override
            public void onActivityResumed(Activity activity) {
                Log.d("wenming", "onActivityResumed");

            }

            @Override
            public void onActivityPaused(Activity activity) {
                Log.d("wenming", "onActivityPaused");
            }

            @Override
            public void onActivityStopped(Activity activity) {
                Log.d("wenming", "onActivityStopped");
                appCount--;
            }

            @Override
            public void onActivitySaveInstanceState(Activity activity, Bundle outState) {
                Log.d("wenming", "onActivitySaveInstanceState");
            }

            @Override
            public void onActivityDestroyed(Activity activity) {
                Log.d("wenming", "onActivityDestroyed");
            }
        });
    }

    public int getAppCount() {
        return appCount;
    }

    public void setAppCount(int appCount) {
        this.appCount = appCount;
    }
}
```

����Ҫ���жϵĵط��������·������ɣ�

``` java
 /**
     * ����3��ͨ��ActivityLifecycleCallbacks������ͳ��Activity���������ڣ������жϣ��˷�����API 14���Ͼ���Ч��������Ҫ��Application��ע��˻ص��ӿ�
     * ���룺
     * 1. �Զ���Application����ע��ActivityLifecycleCallbacks�ӿ�
     * 2. AndroidManifest.xml�и���Ĭ�ϵ�ApplicationΪ�Զ���
     * 3. ��Application��Ϊ�ڴ治�����Kill��ʱ�����������Ȼ������ʹ�á���Ȼȫ�ֱ�����ֵ����˶�ʧ�������ٴν���Appʱ�������ͳ��һ�ε�
     */

    public static boolean getApplicationValue(Context context) {
        return ((MyApplication) ((Service) context).getApplication()).getAppCount() > 0;
    }
```
���������ַ�ʽ��ֻҪ��׽��APP�е���̨�Ķ������Ϳ���������Ҫ���¼������ˣ���ʵ����һ���Ƚϳ��������󣬱���ͨѶ��APP�е���̨��ʱ����Ϣ��notification����ʽpush����������Ƚ�˽��һ���APP�е���̨��ʱ���ٴ��л���Ҫ��������������ȵȡ�


���ܻ������ھ��ᣬ����back���е���̨����Home���е���̨��һ����������������������AndroidӦ�ÿ�����һ����Ϊback���ǿ��Բ���ģ���Home���ǲ��ܲ���ģ������޸�framework��,��������������Activity�����������ֽ�����⣬��Ȼ�����ַ�ʽ��Activity�������ڲ�����ͬ�����Ƕ��߶���ִ��onStop���������Բ������ĵ����Ǵ������ĸ��������̨�ġ�

������:ͨ��ʹ��UsageStatsManager��ȡ
-----

**ԭ��**
ͨ��ʹ��UsageStatsManager��ȡ���˷�����Android5.0֮���ṩ����API�����Ի�ȡһ��ʱ����ڵ�Ӧ��ͳ����Ϣ�����Ǳ�������һ��Ҫ��

**���룺**
  1. �˷���ֻ��android5.0������Ч 
  2. AndroidManifest�м����Ȩ�� 
```  java
<uses-permission xmlns:tools="http://schemas.android.com/tools" android:name="android.permission.PACKAGE_USAGE_STATS" tools:ignore="ProtectedPermissions" />
```
  3. ���ֻ����ã������ȫ-�߼�������Ȩ�鿴ʹ�������Ӧ���У�Ϊ���App���Ϲ�

![enter image description here](https://raw.githubusercontent.com/wenmingvs/NotificationUtil/develop_notify/%E6%9D%83%E9%99%90.PNG)




**�жϺ���**
``` java

 @TargetApi(Build.VERSION_CODES.LOLLIPOP)
    public static boolean queryUsageStats(Context context, String packageName) {
        class RecentUseComparator implements Comparator<UsageStats> {
            @Override
            public int compare(UsageStats lhs, UsageStats rhs) {
                return (lhs.getLastTimeUsed() > rhs.getLastTimeUsed()) ? -1 : (lhs.getLastTimeUsed() == rhs.getLastTimeUsed()) ? 0 : 1;
            }
        }
        RecentUseComparator mRecentComp = new RecentUseComparator();
        long ts = System.currentTimeMillis();
        UsageStatsManager mUsageStatsManager = (UsageStatsManager) context.getSystemService("usagestats");
        List<UsageStats> usageStats = mUsageStatsManager.queryUsageStats(UsageStatsManager.INTERVAL_BEST, ts - 1000 * 10, ts);
        if (usageStats == null || usageStats.size() == 0) {
            if (HavaPermissionForTest(context) == false) {
                Intent intent = new Intent(Settings.ACTION_USAGE_ACCESS_SETTINGS);
                intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                context.startActivity(intent);
                Toast.makeText(context, "Ȩ�޲���\n����ֻ����ã������ȫ-�߼�������Ȩ�鿴ʹ�������Ӧ���У�Ϊ���App���Ϲ�", Toast.LENGTH_SHORT).show();
            }
            return false;
        }
        Collections.sort(usageStats, mRecentComp);
        String currentTopPackage = usageStats.get(0).getPackageName();
        if (currentTopPackage.equals(packageName)) {
            return true;
        } else {
            return false;
        }
    }

    /**
     * �ж��Ƿ�ӵ��PACKAGE_USAGE_STATSȨ��
     */
    @TargetApi(Build.VERSION_CODES.KITKAT)
    private static boolean HavaPermissionForTest(Context context) {
        try {
            PackageManager packageManager = context.getPackageManager();
            ApplicationInfo applicationInfo = packageManager.getApplicationInfo(context.getPackageName(), 0);
            AppOpsManager appOpsManager = (AppOpsManager) context.getSystemService(Context.APP_OPS_SERVICE);
            int mode = appOpsManager.checkOpNoThrow(AppOpsManager.OPSTR_GET_USAGE_STATS, applicationInfo.uid, applicationInfo.packageName);
            return (mode == AppOpsManager.MODE_ALLOWED);
        } catch (PackageManager.NameNotFoundException e) {
            return true;
        }
    }

``` 


�����壺��ȡLinuxϵͳ�ں˱�����/procĿ¼�µ�process������Ϣ
----

**ԭ��**
�����п����������������һ��©����Linuxϵͳ�ں˻��process������Ϣ������/procĿ¼�£�Shell����ȥ��ȡ�������ٸ��ݽ��̵������ж��Ƿ�Ϊǰ̨

**�ŵ�**
1. ����Ҫ�κ�Ȩ��
2. �����ж�����һ��Ӧ���Ƿ���ǰ̨����������������Ӧ��

**�÷�**
��ȡһϵ���������е�App�Ľ���
``` java
List<AndroidAppProcess> processes = ProcessManager.getRunningAppProcesses();
```

��ȡ��һ�������е�App���̵���ϸ��Ϣ
``` java
AndroidAppProcess process = processes.get(location);
String processName = process.name;

Stat stat = process.stat();
int pid = stat.getPid();
int parentProcessId = stat.ppid();
long startTime = stat.stime();
int policy = stat.policy();
char state = stat.state();

Statm statm = process.statm();
long totalSizeOfProcess = statm.getSize();
long residentSetSize = statm.getResidentSetSize();

PackageInfo packageInfo = process.getPackageInfo(context, 0);
String appName = packageInfo.applicationInfo.loadLabel(pm).toString();
```

�ж��Ƿ���ǰ̨
``` java
if (ProcessManager.isMyProcessInTheForeground()) {
  // do stuff
}
```

��ȡһϵ���������е�App���̵���ϸ��Ϣ
``` java
List<ActivityManager.RunningAppProcessInfo> processes = ProcessManager.getRunningAppProcessInfo(ctx);
```

