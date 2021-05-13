# RoomDatabase
RoomDatabase는 SQLite에대한 추상레이어를 제공하고, 기존 SQLite의 많은 보일러플레이트 코드를 제거해줍니다.     
또한 데이터베이스에 더 안정적으로 접근이 가능합니다.
    
### 구성요소
+ Database 데이터베이스 홀더를 포함하고 앱의 지속적인 관계형 데이터 연결을 위한 기본 엑세스 포인트 역할을 합니다.
  + @Database 어노테이션으로 지정하고 RoomDatabase를 확장하는 추상클래스여야 합니다.
  + @Dao 어노테이션이 지정된 클래스를 반환하는 추상 메서드를 포함해야합니다.
+ Entity 데이터베이스 내의 테이블을 나타냅니다.
+ DAO 데이터베이스에 덱세스하는데 사용되는 메서드가 포함됩니다.
    
```kotlin
   @Entity
   data class User(
        @PrimaryKey val uid: Int,
        @ColumnInfo(name = "first_name") val firstName: String?,
        @ColumnInfo(name = "last_name") val lastName: String?
   )
    
    
   @Dao
   interface UserDao {
        @Query("SELECT * FROM user")
        fun getAll(): List<User>

        @Query("SELECT * FROM user WHERE uid IN (:userIds)")
        fun loadAllByIds(userIds: IntArray): List<User>

        @Query("SELECT * FROM user WHERE first_name LIKE :first AND " +
               "last_name LIKE :last LIMIT 1")
        fun findByName(first: String, last: String): User

        @Insert
        fun insertAll(vararg users: User)

        @Delete
        fun delete(user: User)
    }
    
    @Database(entities = arrayOf(User::class), version = 1)
    abstract class AppDatabase : RoomDatabase() {
        abstract fun userDao(): UserDao
    }
    
    
    val db = Room.databaseBuilder(
                applicationContext,
                AppDatabase::class.java, "database-name").build()
```


    
     
     
### 데이터베이스 미리채우기
앱을 개발하다보면 미리 정의된 db를 넣어줘야할 때가 있습니다. 이럴때 createFromAsset 또는 createFromFile 메서드를 활용하면 편하게 db를 넣어줄 수 있습니다.

1. Assets 에서 미리 채우기
     ```kotlin
     Room.databaseBuilder(appContext, AppDatabase.class, "Sample.db")
        .createFromAsset("myapp.db")
        .build()
      ```
      !! 미리넣으려는 db파일은 assets/database 경로에 반드시 저장되어 있어야 합니다.
      
2. File 에서 미리 채우기
    ```kotlin
     Room.databaseBuilder(appContext, AppDatabase.class, "Sample.db")
        .createFromFile(File("mypath"))
        .build()
    ```

      
