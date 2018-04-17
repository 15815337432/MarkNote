
## NotePal--Android�˿�Դ��Markdown�ʼ�Ӧ��

### ���

NotePal��һ�Դ��Markdown�ʼ�Ӧ�ã����Թ��������м���׿�����߽���ѧϰ���ñʼ�Ŀǰ�Ĺ��ܱȽ����ƣ����������һ�������Լ���Markdown�ʼǵ��뷨����ô����Բο�һЩ����Դ���롣��Ȼ���������Դ��Ŀ����ϣ�����������������ḻ����Ĺ��ܣ�������и��������뷨Ҳ������Github�������Ŀ�ύ���롣

������ҴӸ�����Ļ������ܺ���Щ���ܵ�ʵ�ַ�ʽ�����������һ�����Ӧ�á��������ȣ����ǻ�������һ�¸������һЩ��ͼ��

### ���ֽ�ͼ

![Screenshots](https://github.com/Shouheng88/NotePal/blob/master/screenshots/note_view_page.png)

![Screenshots](https://github.com/Shouheng88/NotePal/blob/master/screenshots/note_view_page_2.png)

��ͼ�������������Materiald Design����Ʒ�������漰����һЩ������֧�ְ���Ŀؼ�����ȻҲ������Github����һЩ��Դ�Ŀ⡣�������һ����ѧ�ߣ����Ҷ�Material Design����Ȥ�Ļ��������ο�һ�����Ĵ��롣

### ��������

* ������**����**, **�鵵**, **�Ž�������**��**����ɾ��** ����
* Markdown�﷨֧�֣�����: **����, �����б�, ��ѡ��, ���½ű�, �Ӵ�, ��б, ��ѧ��ʽ, ���, ѡ��, ͼƬ�����ӵ�**
* **ʱ����**������¼�ڳ����еĲ������ر���һЩ���ݿ�Ĳ���
* **�ļ�, ��Ƶ, ��Ƶ, ͼƬ, ��д, λ��**�Լ������Ķ�ý��֧��
* ��ʵ�**ͼ��**����ͳ���û���Ϣ
* ������֧�֣�������**��ҹ������, 13������ɫ, 16��ǿ��ɫ�Լ��Ƿ�Ե�������ɫ**
* **����С�ؼ�**�����б�͹��������Լ�**�����ݷ�ʽ**
* ��ʵ�**��ǩ**������ѡ��ͼ�����ɫ
* ���ֱʼǹ���ʽ��������ǩ��**�㼶�ṹ**
* ���ֱʼǵ�����ʽ��������**PDF, txt, md�Լ�html**
* **Ӧ�ö�����**
* **���ݵ��ⲿ�洢�豸**�Լ�**���ݵ�OneDrive**
* **ͼƬѹ��**

### ����ʵ��

#### 1.���ݿ����

��������Ľ��ܣ���������ʹ���˱�ǩ�Ͳ㼶�ṹ���ֹ���ʽ��˳�㻹��ʱ���ߵĹ��ܣ����������Ƚ���һ�����ݿ�����ơ�

����������е����ݿ������̳���Model�࣬�����ж����˻���������Ϣ��ע���ڳ������������������Զ����ע��Column��Table���ֱ�����ָ�����ݿ�������ͱ�����

```

	public class Model implements Serializable {

		@Column(name = "id")
		protected long id;

		@Column(name = "code")
		protected long code;

		@Column(name = "user_id")
		protected long userId;

		// ....
	}
	
	@Table(name = "gt_note")
	public class Note extends Model implements Parcelable {

		@Column(name = "parent_code")
		private long parentCode;
		
		// ....		
	}

```

������ʾ��������Model����Columnָ�������е����ݿ������Ҫ���ֶΡ�Ȼ�󣬶Աʼ�����Note�������̳�Model��������Tableָ�������ı����������Note�ж�����һЩ�ʼǶ�������ݿ���Ϣ��

˵�������ݿ�������ǿ�һ�����ݿ��ѯ������Ϊ�����ǵĳ�������Ҫ�����ݿ��ѯ�ȵķ�����һЩ����Ĳ���������û��ֱ��ʹ�ÿ�Դ�����ݿ�Ŀ⣬����Room�ȣ������Լ������һ�ס������ǵ����ݿ������Ҳ��һЩ�����ĵط����������ǿ���ֻ���м򵥵����þ������һ���µĶ�������ݿ��ѯ������������Ϊ������κ�SQL���������ݿ������õ��Ķ���ԭ��Android���ݿ��֪ʶ���������������ʵ�Ļ���������⡣

public class PalmDB extends SQLiteOpenHelper {

    public static final String DATABASE_NAME = "NotePal.db";
    private static final int VERSION = 7;

    private Context mContext;
    @SuppressLint("StaticFieldLeak")
    private static PalmDB sInstance = null;

    private volatile boolean isOpen = true;

    public static PalmDB getInstance(final Context context){
        if (sInstance == null){
            synchronized (PalmDB.class) {
                if (sInstance == null) {
                    sInstance = new PalmDB(context.getApplicationContext());
                }
            }
        }
        return sInstance;
    }

    private PalmDB(Context context) {
        super(context, DATABASE_NAME, null, VERSION);
        this.mContext = context;
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        NotesStore.getInstance(mContext).onCreate(db);
        // ... 
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        NotesStore.getInstance(mContext).onUpgrade(db, oldVersion, newVersion);
		// ...
    }
}

������ʾ��������PalmDB�̳���SQLiteOpenHelper����������onUpgrade��onCreate���ø�����ѯ����ĸ��ºʹ�����������������һ�������Ķ�����Ϊһ�����ݿ�ֻҪ��һ��SQLiteOpenHelper�͹��ˣ���������ֻҪһ�������ľ͹��ˡ�


