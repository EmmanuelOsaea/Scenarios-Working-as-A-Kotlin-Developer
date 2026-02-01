









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




