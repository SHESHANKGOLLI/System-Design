@PostMapping(alue = "/user", consumes = MediaType.APPLICATION_JSON_VALUE, produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<Map<String, Object>> createUser(@Valid @RequestBody UserRegistrationDto userDto) {
        // Production logic would save the user here
        Map<String, Object> response = new LinkedHashMap<>();
        response.put("timestamp", new Date());
        response.put("status", "SUCCESS");
        response.put("message", "User registered successfully");
        response.put("userId", UUID.randomUUID().toString());
        
        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }


@GetMapping("/user/{id}")
@ResponseBody // (Redundant if using @RestController)
public User getUser(@PathVariable Long id) {
    // Always returns HTTP Status 200 OK along with the JSON data
    return userService.findById(id); 
}


@PostMapping("/user")
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    User savedUser = userService.save(user);
    
    return ResponseEntity
            .status(HttpStatus.CREATED) // Returns 201 Created
            .header("X-Custom-Header", "UserManagement") // Custom Header
            .body(savedUser); // JSON Body
}

@GetMapping("/user/{id}")
public ResponseEntity<Map<String, Object>> getUserSecurely(@PathVariable Long id) {
    Optional<User> user = userService.findById(id);
    
    if (user.isEmpty()) {
        // Dynamically change status to 404 if data missing
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                             .body("User not found");
    }
    return ResponseEntity.ok(user.get()); // Returns 200 OK
}


//This is used when you want file and also a json object.

1.Under the Body tab, select form-data.
2.Add a key named file, change its type to File, and select your file.
3: Add a key named user, leave its type as Text, and paste your raw JSON (e.g., {"username":"john_doe", "email":"john@example.com", ...}).
4: Crucial Step: Click the three dots (or hidden settings) on the far right of the user row in Postman, enable Content Type, and explicitly type application/json.

@PostMapping(
    value = "/upload", 
    consumes = MediaType.MULTIPART_FORM_DATA_VALUE, 
    produces = MediaType.APPLICATION_JSON_VALUE
)
public ResponseEntity<Map<String, Object>> uploadToS3(
        @RequestPart("file") MultipartFile file, 
        @Valid @RequestPart("user") UserRegistrationDto userDto,  
        @RequestParam(value = "executionMode", defaultValue = "EVENT") String executionMode) {
    
        Map<String, Object> response = new LinkedHashMap<>();
        response.put("timestamp", new Date());
        response.put("status", "SUCCESS");
        response.put("message", "User registered successfully");
        response.put("userId", UUID.randomUUID().toString());

        //lets say u wanted to return OK
        return ResponseEntity.status(ACCEPTED).body(Map.of("Status","Success"));
        return ResponseEntity.status(CREATED).body(response);
        return ResponseEntity.internalServerError().body(Map.of("Status","Failed"));
        return ResponseEntity.badRequest().body(Map.of("Status","Failed"));

        //if returnType is void ResponseEntity<Void>
        //return ResponseEntity.accepted().build();
        //return ResponseEntity.internalServerError().build();
        //return ResponseEntity.badRequest().build();
        //return ResponseEntity.status(ACCEPTED).build();
        //return ResponseEntity.status(CREATED).build();

         final String filename = file.getOriginalFilename();
         final String fileExtension = getFilenameExtension(filename);//comes from stringutils , default method
         InputStream inputStream = file.getInputStream();
         asyncTaskExecutorHelper.invokeAsyncCall(() -> {
            try (InputStream asyncInputStream = inputStream) { // Closing inside the async task
                bulkMigrationService.processCsvFile(asyncInputStream, executionMode, migrationType);
            } catch (IOException e) {
                log.error("Error processing CSV file: {}, error message: {}", filename, e.getMessage());
                throw exceptionHelper.internalServerException(e.getMessage());
            }
        });
} 



public static class UserRegistrationDto {
        
    @NotBlank(message = "Username cannot be empty or blank")
    @Size(min = 4, max = 20, message = "Username must be between 4 and 20 characters")
    private String username;

    @NotBlank(message = "Email is required")
    @Email(message = "Please provide a valid email address")
    private String email;

    @NotBlank(message = "Password is required")
    @Pattern(
        regexp = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z]).{8,}$",
        message = "Password must be at least 8 characters long and contain at least one digit, one uppercase, and one lowercase letter"
    )
    private String password;

    @NotNull(message = "Age is required")
    @Min(value = 18, message = "User must be at least 18 years old")
    @Max(value = 120, message = "Age cannot exceed 120")
    private Integer age;

    @Past(message = "Date of birth must be a past date")
    private LocalDate dateOfBirth;

    @NotEmpty(message = "At least one user role must be specified")
    private List<String> roles;

}



