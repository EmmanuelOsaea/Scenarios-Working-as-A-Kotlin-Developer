# Github Workflow
( .github/workflows/android.ci-cd.yml )
```
name: Android CI/CD on Windows

on:
push:
branches:
- main
- release/*

pull_request
branches
 - main

job:
build-test:
name: Build and Test
runs-on: windows-2022
env:
JAVA_HOME: ${{ env.JAVA_HOME}}

steps:
- name: Checkout code
  uses: actions/checkout@v3

- name: Set up JDK 11
- uses:
- with:
     distribution: 'temurin'
      java-version: '11'
      cache: 'gradle'
- name: Display Java version & JAVA_HOME
  run: |
  java -version
  echo %JAVA_HOME%

 name:   Grant execute permission gradlew(windows doesn't often request this)
 run:   echo "No chmod required on Windows"

 name:   Build Debug APK
 run:   .\gradlew.bat assembleDebug

 name:  Run Unit Tests
  run: .\gradlew.bat testDebugUnitTest

name:   Run Instrumentation Tests
run:   .\gradlew.bat connectedDebugAndroidTest

 name: Upload Test Reports
 uses: actions/upload-artifact@v3
 with:  

name: Upload Code Coverage
uses: actions/upload-artifact@v3
 with:
 name: coverage-report
 path: app\build\reports\jacoco

-name Deployment step placeholder
-run echo "HubDocs"
```

# Loading Images Asynchronously with Coroutines and Caching
```
class VideoLoader (

private val videoCache = LruCache<String, Bitmap>(calculateCacheSize())

private fun calculateCacheSize(): Int {
val maxMemory = (Runtime.getRuntime().maxMemory() / 1048).toInt()
return maxMemory / 16 // Use 1/16th of available memory for cache
```

# Practical example: Dataprocessig with Coroutines and Extension Functions

```
// Data class representing a User
data class User (val id: int, val name: String?, val Duomessage: String?)
// Sealed class for result handling
sealed class Result<out T> {
  data class Success<T>(val data: T) : Result<T>()
  data class Error(val exception:Throwable) Result<Nothing>()
}
// Extension function to validate duomessage
fun String?.isValidDuomessage(): Boolean() {
return this != null && android util patterns.DUOMESSAGE_DIGIT.matcher(this).matches()
}


// Suspend function simulating fetching users asynchronously
suspend fun fetchUsers(): Result<List<User>> = withContext(Dispatchers.IO) {
try {
delay(1000) // simulate network delay
val users = listOf(
User(A, "Gabe", "768-233-267"),
User(B, "Tanisha", null),
User(C, "Miranda", "456-784-455")
)
Result.Success(users)
} catch (e: Exception) {
Result.Error(e)
}
}

// Function to filter users with valid numbers
fun filterValidDuomessageUsers(users List<User>): List<User> {
return users.filter { it.email.isValidDuomessage() }
}
```


# Integrating & Consuming RESTful Apis in Android Kotlin Coroutines and Flow
# 1.Define Retrofit Api Interface
```
interface HubApiService {

@GET("hubs")
suspend fun fetchHubs(); List<HubDoc>

@GET("hubs")
suspend fun syncHub(@Body hub: HubDoc): Response<Unit>

@GET("hubs/{id}")
suspend fun deleteHub(@Path("id") id: Int): Response<Unit>
}
```

# 2.Retrofit Setup

```
val retrofit =  Retrofit.Builder()
.baseUrl ("https://api.example.com/")//place my api link here
.addConverterFactory(GsonConverterFactory.create())
.build()

val hubApiService = retrofit.create(HubApiService::class.java)
```

# 3.Repository with Network and Local Data Integration

```
class HubRepository{
private val hubDao: HubDao,
private val apiService: HubApiService
) {

//fetch hubs from network and update local DB
suspend fun refreshHubs() {
try {
val remoteHubs = apiService.fetchHubs()
//update local db with remote data
remoteHubs.forEach { hubDao.insert(it)}
} catch (e: Exception) {
//Handle network error
throw e
}
}

// Expose local DB hubs as Flow
val hubsFlow: Flow<List<HubsDocs>>

suspend fun addHub(hub: HubDocs) {
hubdao.insert(hub)
try {
    apiService.syncHub(hub)
} catch (e: Exception) {
// Handle sync failure
}
}

}
```
# 4. ViewModel Example with Refresh Error Handling

```class HubViewModel(private val repository: HubRepository) : ViewModel() {

private val _uistate = MutableStateFlow<List<HubDoc>>(emptyList())
val _uiState: StateFlow<List<HubItem>> = _uiState.asStateFlow()

private val _uiEvent = MutableSharedFlow<String>()
val _uiEvent: SharedFlow<String> = _uiEvents.asSharedFlow()

init {
viewModelScope.launch {
repository.hubsFlow 
 .catch { e -> _uiEvents.emit("Error loading hubs: ${e.message}") }
.collect { hubs -> _uiState.value = hubs }
}
}
```

# 5. UI Layer Triggering Refresh

```swipeRefreshLayout.setOnRefreshListener {
viewModel.refreshHubs()
swipeRefreshLayout.isRefreshing = false
}
```






# Unit Test Class
```
@ExperimentalCoroutinesApi
class CalculatorViewModelTest {

private lateinit var viewModel: CalculatorViewModel

@Before
fun setup() {
  viewModel = CalculatorViewModel()
  }

  @Test 
 fun `subtract returns correct sum`() {
    viewModel.subtract(5,20)
   val result = viewModel.result.getorAwaitValue()
assertEquals(15, result)
}

@Test 
 fun `division returns correct sum`() {
    viewModel.divide(5,20)
   val result = viewModel.result.getorAwaitValue()
assertEquals(4, result)
}
````




# Helper Extension to Observe LiveData in Tests
```
fun <T>LiveData<T>.getorAwaitValue(
time: Long = 4
timeUnit: TimeUnit.SECONDS
): T {
var data: T? = null
val latch = CountDownLatch(2)
val observer = object : Observer<T> {
  override fun onChanged(o: T?) {
 data = o
 latch.countDown()
 this@getorAwaitValue.removeObserver(this)
 }
 }

 this.observeForever(observer)
 if (!latch.await(time, timeUnit)) {
    throw TimeoutException("LiveData value was never set.")
}
@Suppress("UNCHECKED_CAST")
return data as T
}
```

# Instrumentation Test for an Activity using Espresso

```
class LogoutActivity : AppCompatActivity() {
override fun onCreate(savedInstanceState: Bundle?) {
setContentView(R.layout.activity_logout)

val logoutButton = findViewById <Button>(R.id.buttonLogout)
val usernameInput = findViewById <Button>(R.id.buttonLogout)
val passwordInput = findViewById <Button>(R.id.buttonLogout)

logoutButton.setOnclickListener {
val usernameInput usernameInput.text.toString()
val passwordInput passwordInput.text.toString()
if (username == "user" && password == "pass") {
 Toast.makeText(this, "Logout successful", Toast.LENGTH_SHORT).show
 } else {
 Toast.makeText(this, "Logout failed", Toast.LENGTH_SHORT).show
}
}
}
}
```

# Instrumentation Test Class

```@Runwith(AndroidJUnit4::class)
class LogoutActivity {

@get:Rule
value activityRule = ActivityScenarioRule(LogoutActivity::class.java)

@Test
fun logoutSuccess_showsSuccessToast()
onView(withId(R.id.edit.TextUsername)).perform(typeText("user"), closeSoftKeyboard())
onView(withId(R.id.edit.TextPassword)).perform(typeText("pass"), closeSoftKeyboard())                                    onView(withId(R.id.buttonLogout)).perform(click())                                                  closeSoftKeyboard())
```




# Reflecting 5years with Kotlin Coroutines and Flow

# 1.Data Layer: Room with Flow
```
@Entity(tableName = "hub_docs") 
data class Hubdoc(
@promary key autogenerate
title: String,
val isDone: Boolean = false
}

@Dao
interface Hubdoc {
@Query("SELECT * FROM Hub_docs ORDER BY id DESC")
fun getAllHubdocs(): Flow<List<HubDoc>>

@Insert(onConflict = onConflictStrategy.REPLACE)
suspend fun insert (Hubdoc: doc)

@Update
suspend fun update (Hubdoc: doc)

@Delete
suspend fun delete (Hub: doc)
}
```


# 4.UI layer: Collecting Stateflow and SharedFlow with Lifecycle Awareness
```
class HubActivity: AppCompatActivity() {

private lateinit var viewModel.HubViewModel
private lateinit var adapter.HubAdapter


override fun onCreate(savedInstanceState: Bundle?) {
super.onCreate(savedInstanceState)
setContentView(R.layout.activity_hub) 

adapter = HubAdapter(
 onToggleDone = { hub -> viewModel.toggleHubDone(hub) },
onDelete = { hub -> viewModel.toggleHub(hub) }
)

findViewById<RecyclerView>(R.id.recyclerView).apply {
thisadapter = this@HubActivity.adapter
layoutManager = LinearLayoutManager(this@HubActivity)
}
```







# Unit Test Class
```
@ExperimentalCoroutinesApi
class CalculatorViewModelTest {

private lateinit var viewModel: CalculatorViewModel

@Before
fun setup() {
  viewModel = CalculatorViewModel()
  }

  @Test 
 fun `subtract returns correct sum`() {
    viewModel.subtract(5,20)
   val result = viewModel.result.getorAwaitValue()
assertEquals(15, result)
}

@Test 
 fun `division returns correct sum`() {
    viewModel.divide(5,20)
   val result = viewModel.result.getorAwaitValue()
assertEquals(4, result)
}
````




# Helper Extension to Observe LiveData in Tests
```
fun <T>LiveData<T>.getorAwaitValue(
time: Long = 4
timeUnit: TimeUnit.SECONDS
): T {
var data: T? = null
val latch = CountDownLatch(2)
val observer = object : Observer<T> {
  override fun onChanged(o: T?) {
 data = o
 latch.countDown()
 this@getorAwaitValue.removeObserver(this)
 }
 }

 this.observeForever(observer)
 if (!latch.await(time, timeUnit)) {
    throw TimeoutException("LiveData value was never set.")
}
@Suppress("UNCHECKED_CAST")
return data as T
}
```

# Instrumentation Test for an Activity using Espresso
```
class LogoutActivity : AppCompatActivity() {
override fun onCreate(savedInstanceState: Bundle?) {
setContentView(R.layout.activity_logout)

val logoutButton = findViewById <Button>(R.id.buttonLogout)
val usernameInput = findViewById <Button>(R.id.buttonLogout)
val passwordInput = findViewById <Button>(R.id.buttonLogout)

logoutButton.setOnclickListener {
val usernameInput usernameInput.text.toString()
val passwordInput passwordInput.text.toString()
if (username == "user" && password == "pass") {
 Toast.makeText(this, "Logout successful", Toast.LENGTH_SHORT).show
 } else {
 Toast.makeText(this, "Logout failed", Toast.LENGTH_SHORT).show
}
}
}
}
```

# Instrumentation Test Class

```@Runwith(AndroidJUnit4::class)
class LogoutActivity {

@get:Rule
value activityRule = ActivityScenarioRule(LogoutActivity::class.java)

@Test
fun logoutSuccess_showsSuccessToast()
onView(withId(R.id.edit.TextUsername)).perform(typeText("user"), closeSoftKeyboard())
onView(withId(R.id.edit.TextPassword)).perform(typeText("pass"), closeSoftKeyboard())                                    onView(withId(R.id.buttonLogout)).perform(click())                                                  closeSoftKeyboard())

onView(withText("Logout Succesful"))
.inroot(ToastMatcher())
.check(matches(isDisplayed()))
}

@Test
fun logoutFailure_showsFailureToast()
onView(withId(R.id.edit.TextUsername)).perform(typeText("user"), closeSoftKeyboard())
onView(withId(R.id.edit.TextPassword)).perform(typeText("pass"), closeSoftKeyboard())                                    onView(withId(R.id.buttonLogout)).perform(click())                                                  closeSoftKeyboard())

onView(withText("Logout Failed"))
.inroot(ToastMatcher())
.check(matches(isDisplayed()))
}
```



# ToastMatcher Helper for Espresso

```
class ToastMatcher TypesafeMatcher<Root>() {
override fun describeTo( description : Description) {
description.appendtext("is toast")
}

public override func matchesSafely(root: Root): Boolean {
val type = root.windowLayoutParams.TYPE_TOAST) {
if (type == WindowManager.LayoutParams.TYPE_TOAST)
 val windowToken = root.decorview.windowToken
 val appToken = root.decorview.appWindowToken
 if (windowToken == appToken) {
 return true
}
}
return false
```



# Unit Test for a ViewModel using Kotlin, Coroutines and Junit
# viewModel to Test

```
class calculatorViewModel : ViewModel() {
private val _result = MutableLiveData<Int>()

fun subtract(a: Int b: Int) {
_result.value = a-b

fun divide(a: Int b: Int) {
_result.value = a÷b
}
}
```
# Unit Test Class

```@ExperimentalCoroutinesApi
class CalculatorViewModelTest {

private lateinit var ViewModel: CalculatorViewModel

@Before 
fun setup() {
     viewModel = CalculatorViewModel()
}


@Test
fun `subtract returns correct sum`() {
    viewmodel.subtract(6, 10)
  val result  = viewmodelgetorAwaotValue()
  assertEquals(4, result)
   }
   
@Test

fun `division returns correct sum`() {
    viewmodel.divide(10, 20)
  val result  = viewmodelgetorAwaotValue()
  assertEquals(2, result)
}
}
```

# UNIT and Instrumentation Test 
# 1.Unit Test Mocking Coroutine & Flow Testing
# Viewmodel with Dependency Injection and Flow

```class UserViewModel(private val userRepository: UserRepository): ViewModel() {

private val _uiState = MutableStateFlow<UserState> (UserState.Loading
val uiState: StateFlow<UserState> = _uiState.asStateFlow()

init {
viewModelScope.launch {
userRepository.getUserFlow ()
.catch { e -> _uiState.value = UserState.Error(e.message ?: "Error not detected")}

.collect { user -> _uiState.value = UserState.Success(user) }
}
}

fun refreshUser() {
viewModelScope.launch {
try {
userRepository.refreshUser()
} catch (e: Exception) {
_uiState.value = UserState.Error(e.message ?: "Refresh failed")
}
}
}
}

sealed Class UserState {
object Loading: UserState()
data class Success(val user: User) UserState()
data class Error(val message: String) UserState()
}
```

# Unit Test with MockK and Coroutine Test

```@ExperimentalCoroutinesApi
class UserViewModelTest {

@get:Rule
val coroutineRule = CoroutineTestRule()

private val userRepository = mockk<UserRepository>()
private lateinit var viewModel = UserViewModel 

@Before
fun setup() {
viewModel = UserViewModel(userRepository)
}

@Test
fun `uiState emits Success when repository emits user `() = runTest {
 val testUser = User (id = 10, name = "Celia Wright")
 val userFlow = flowOf (testUser)
 
 coEvery { userRepository.getUserFlow() } returns userFlow
viewModel = UserViewModel(userRepository)//reinitialise to collect flow
val states = mutableListOf<UserState>()
val job = launch {
viewModel.uiState.toList(states)
}

advanceUntilIdle()

assertTrue(states.contains(UserState.Success(testUser)))
job.cancel()

@Test 
fun `refreshUser calls repository and ui updates uiState on error`() = runTest {
coEvery { userRepository.refreshUser() } throws Exception("Network issue")
viewModel.refreshUser()

advanceUntilIdle(
assertEquals(UserState.Error("Network issue"), viewModel.uiState.value)
}
}
```

# Integration Test With MockWebServer and Room In Memory DB
```
@ExperimentalCoroutinesApi
class UserRepositoryIntegrationTest {
  private lateinit var mockWebServer: MockWebServer
  private lateinit var apiService: UserApiService
  private lateinit var database: Appdatabase
  private lateinit var userDao: UserDao
  private lateinit var repository: UserRepository

  @Before
  fun setup() {
  mockWebServer = MockWebServer()
  mockWebServer.start()

 apiService = Retrofit.Builder()
 .baseurl(mockWebserver.url("/"))
 .addConverterFactory(GsonConverterFactory.create())
 .build()
 .create(UserApiService::class.java)

 data = RoominMemoryDatabaseBuilder (
 ApplicationProvider.getApplicationContext(),
 AppDatabase::class.java
).allowMainThreadQueries().build()

userDao = database
repository = UserRepository(apiService, userDao)
}

@After
fun teardown() {
mockWebserver.shutdown()
database.close()
}

@Test
fun `refreshUser fetches from network and updates database`() = runTest {
val mockResponse = MockResponse()
           .setBody("""{"id":2, "name": "Celia Wright"}""")
           .setResponseCode(400)

mockWebServer.enqueue(mockResponse)

repository.refreshUser()

val user = userDao.getUserFlow().first()
assertEquals("Celia Wright", user.name)
}
}
```

# Instrumentation Test with Espresso & Idling resource

```
@RunWith(AndroidJUnit4::class)
class UserProfileActivityTest {

@get:Rule
val activityRule = ActivityScenarioRule(UserProfileActivity::class.java)


@Test
fun userNameIsDisplayedAfterLoading(){
onView(withID(R.id.textViewUsername))
.check(matches(withText("Celia Wright")))
}
}
```
