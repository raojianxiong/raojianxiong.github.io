---
layout: post
title:  "GreenDAO使用及升级"
category: Android
date:   2017-07-19 20:15:48
categories: Android

---


![143506vrdrr0ooxwkxxrr0.jpg](http://upload-images.jianshu.io/upload_images/4105122-0f18cd37f08d8e8f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/320)

GreenDAO 3.2.0：
1、在工程的build.gradle中做如下配置：
```
buildscript {
    dependencies {
        classpath 'org.greenrobot:greendao-gradle-plugin:3.0.0'
    }
}
```
2、在app的build.gradle中做如下配置
```
apply plugin: 'org.greenrobot.greendao'

android {
      greendao{
        schemaVersion 1 // 当前数据库版本号，升级数据库则+1。
        daoPackage 'com.example.greendao.gen' // 生成数据库文件的具体包
        targetGenDir 'src/main/java' // 生成数据库文件的目录
    }
}

dependencies {
    compile 'org.greenrobot:greendao:3.2.0' // GreenDao在github上的依赖地址
}
```
3、创建Model类，然后，Build - Make Module 'app'
```
@Entity(nameInDb = "userinfo")
public class UserInfo implements Serializable {

    @Id(autoincrement = true)
    private Long _id;

    @Property(nameInDb = "username")
    private String username;

    @Property(nameInDb = "age")
    private int age;
    ......
}
```
4、GreenDAO的使用
    ok，上面的是准备工作，接下来才是使用，即得到数据库对象进行增、删、改、查。然后根据自己的代码习惯，做封装。具体实现这里不做叙述，如果有问题欢迎留言。
推荐一篇介绍得很详细的文章，[链接地址](http://www.jianshu.com/p/4986100eff90)。

5、GreenDAO升级
    这个也是我写这篇文章的原因，在完成上面的1-4步后，数据库增删改查都没有问题，愉快地commit and push，愉快地拿着工资吃着火锅唱着歌。
    直到某一天，你在你的Model类即userinfo中加入“test”字段，你很愉快地加上了字段，在build中使greendao - schemaVersion +1，并运行......
![表中没有test字段](http://upload-images.jianshu.io/upload_images/4105122-ea577fbf2c116259.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由于你加了新的字段，表发生了变化，做数据库操作时抛出异常，找不到test字段。那么应该以怎么样的姿势新增字段和升级数据库呢？
然后各种查资料搜索，找到了以下代码，完美解决数据库升级问题。其原理也是创建临时数据库来做数据迁移。
```
import android.database.Cursor;
import android.text.TextUtils;
import android.util.Log;
import org.greenrobot.greendao.AbstractDao;
import org.greenrobot.greendao.database.Database;
import org.greenrobot.greendao.internal.DaoConfig;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class MigrationHelper {

    private static final String CONVERSION_CLASS_NOT_FOUND_EXCEPTION = "MIGRATION HELPER - CLASS DOESN'T MATCH WITH THE CURRENT PARAMETERS";
    private static MigrationHelper instance;

    public static MigrationHelper getInstance() {
        if (instance == null) {
            instance = new MigrationHelper();
        }
        return instance;
    }

    public void migrate(Database db, Class<? extends AbstractDao<?, ?>>... daoClasses) {

        generateTempTables(db, daoClasses);
        DaoMaster.dropAllTables(db, true);
        DaoMaster.createAllTables(db, false);
        restoreData(db, daoClasses);
    }

    /**
     * 生成临时列表
     * @param db
     * @param daoClasses
     */
    private void generateTempTables(Database db, Class<? extends AbstractDao<?, ?>>... daoClasses) {
        for (int i = 0; i < daoClasses.length; i++) {
            DaoConfig daoConfig = new DaoConfig(db, daoClasses[i]);

            String divider = "";
            String tableName = daoConfig.tablename;
            String tempTableName = daoConfig.tablename.concat("_TEMP");
            ArrayList<String> properties = new ArrayList<>();

            StringBuilder createTableStringBuilder = new StringBuilder();

            createTableStringBuilder.append("CREATE TABLE ").append(tempTableName).append(" (");

            for (int j = 0; j < daoConfig.properties.length; j++) {
                String columnName = daoConfig.properties[j].columnName;

                if (getColumns(db, tableName).contains(columnName)) {
                    properties.add(columnName);

                    String type = null;

                    try {
                        type = getTypeByClass(daoConfig.properties[j].type);
                    } catch (Exception exception) {
                        exception.printStackTrace();
                    }

                    createTableStringBuilder.append(divider).append(columnName).append(" ").append(type);

                    if (daoConfig.properties[j].primaryKey) {
                        createTableStringBuilder.append(" PRIMARY KEY");
                    }

                    divider = ",";
                }
            }
            createTableStringBuilder.append(");");

            db.execSQL(createTableStringBuilder.toString());

            StringBuilder insertTableStringBuilder = new StringBuilder();

            insertTableStringBuilder.append("INSERT INTO ").append(tempTableName).append(" (");
            insertTableStringBuilder.append(TextUtils.join(",", properties));
            insertTableStringBuilder.append(") SELECT ");
            insertTableStringBuilder.append(TextUtils.join(",", properties));
            insertTableStringBuilder.append(" FROM ").append(tableName).append(";");

            db.execSQL(insertTableStringBuilder.toString());

        }
    }

    /**
     * 存储新的数据库表 以及数据
     * @param db
     * @param daoClasses
     */
    private void restoreData(Database db, Class<? extends AbstractDao<?, ?>>... daoClasses) {
        for (int i = 0; i < daoClasses.length; i++) {
            DaoConfig daoConfig = new DaoConfig(db, daoClasses[i]);
            String tableName = daoConfig.tablename;
            String tempTableName = daoConfig.tablename.concat("_TEMP");
            ArrayList<String> properties = new ArrayList();

            for (int j = 0; j < daoConfig.properties.length; j++) {
                String columnName = daoConfig.properties[j].columnName;

                if (getColumns(db, tempTableName).contains(columnName)) {
                    properties.add(columnName);
                }
            }

            StringBuilder insertTableStringBuilder = new StringBuilder();

            insertTableStringBuilder.append("INSERT INTO ").append(tableName).append(" (");
            insertTableStringBuilder.append(TextUtils.join(",", properties));
            insertTableStringBuilder.append(") SELECT ");
            insertTableStringBuilder.append(TextUtils.join(",", properties));
            insertTableStringBuilder.append(" FROM ").append(tempTableName).append(";");

            StringBuilder dropTableStringBuilder = new StringBuilder();
            dropTableStringBuilder.append("DROP TABLE ").append(tempTableName);
            db.execSQL(insertTableStringBuilder.toString());
            db.execSQL(dropTableStringBuilder.toString());
        }
    }

    private String getTypeByClass(Class<?> type) throws Exception {
        if (type.equals(String.class)) {
            return "TEXT";
        }
        if (type.equals(Long.class) || type.equals(Integer.class) || type.equals(long.class)) {
            return "INTEGER";
        }
        if (type.equals(Boolean.class)) {
            return "BOOLEAN";
        }

        Exception exception = new Exception(CONVERSION_CLASS_NOT_FOUND_EXCEPTION.concat(" - Class: ").concat(type.toString()));
        exception.printStackTrace();
        throw exception;
    }

    private List<String> getColumns(Database db, String tableName) {
        List<String> columns = new ArrayList<>();
        Cursor cursor = null;
        try {
            cursor = db.rawQuery("SELECT * FROM " + tableName + " limit 1", null);
            if (cursor != null) {
                columns = new ArrayList<>(Arrays.asList(cursor.getColumnNames()));
            }
        } catch (Exception e) {
            Log.v(tableName, e.getMessage(), e);
            e.printStackTrace();
        } finally {
            if (cursor != null)
                cursor.close();
        }
        return columns;
    }
}
```
然后只需要继承DaoMaster.OpenHelper重写onUpgrade方法
```
public class SQLiteOpenHelper extends DaoMaster.OpenHelper {

    public SQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory) {
        super(context, name, factory);
    }

    @Override
    public void onUpgrade(Database db, int oldVersion, int newVersion) {
        MigrationHelper.getInstance().migrate(db, UserInfoDao.class);

    }
}
```
最后在初始化DaoMaster处调用。
