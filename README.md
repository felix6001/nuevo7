# nuevo7
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
val retrofit = Retrofit.Builder()
    .baseUrl(BASE_URL) // Reemplaza BASE_URL con la URL base del API de Instagram
    .addConverterFactory(GsonConverterFactory.create())
    .build()

val apiService = retrofit.create(ApiService::class.java)
interface ApiService {
    @GET("users/{userId}/photos")
    suspend fun getUserPhotos(@Path("userId") userId: String): Response<List<Photo>>
}
suspend fun fetchUserPhotos(userId: String): List<Photo>? {
    val response = apiService.getUserPhotos(userId)
    if (response.isSuccessful) {
        return response.body()
    }
    return null
}
data class Photo(
    @SerializedName("id")
    val id: String,
    @SerializedName("url")
    val url: String,
    // Otros campos de la foto
)
