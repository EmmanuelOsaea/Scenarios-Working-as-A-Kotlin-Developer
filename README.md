




# Reflecting 5years with Kotlin Coroutines and Flow

# 1.Data Layer: Room with Flow

@Entity(tableName = "hub_docs") 
data class Hubdoc(
@promary key autogeberate
val title: String,
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


# 2.Repository Layer: Combining Local DB and Network Sync






# 3.ViewModelLayer: StateFlow  and SharedFlow for UI State and Events




# 4.UI layer: Collecting Stateflow and SharedFlow with Lifecycle Awareness

class HubActivity: AppCompatActivity() {

private lateinit var viewModel.HubViewModel
private lateinit var adapter.HubAdapter


override fun onCreate(savedInstanceState: Bundle?) {
super.onCreate(savedInstanceState)
setContentView(R.layout.activity_hub) 

















# Integrating & Consuming RESTful Apis in Android Kotlin Coroutines and Flow
















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

# Instrumemtation Test for an Activity using Espresso

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


# Instrumemtation Test Class

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
}```

# Integration Test With MockWebServer and Room In Memory DB

```@ExperimentalCoroutinesApi
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
