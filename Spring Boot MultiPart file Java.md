@RestController
@RequestMapping("/upload")
public class FileUploadController {

	Map<String, Integer> fileStatus = new ConcurrentHashMap<String, Integer>();

	@PostMapping(value = "/file", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
	public ResponseEntity<String> uploadFile(HttpServletRequest request, MultipartFile file) {
		
		private static final String uuid =. UUID.randomUUId().toString();

		if(file == null)
		{
		return ResponseEntity.status(HTTP_STATUS.Failed).build();
		}

		String fileName = file.getOriginalFilename();
		String fileExtension = StringUtils.getFilenameExtension(fileName);

		byte[] buffer = new byte[8192]; //First we are Allocates an 8 Kilobyte byte array container in memory.
		long totalBytesRead = 0; // Initializes a tracking variable to count how many total bytes have moved from the network to the disk.
            	long fileSize = file.getSize(); // WHAT: Pulls the total file payload size directly from the MultipartFile object framework wrapper mapping metadata.
		int bytesRead; // WHAT: Pre-allocates a temporary chunk index pointer variable to capture the length of the data fragment processed within the active execution loop.

		try(InputStream inputStream = file.getInputStream())	{
			FileOutputStream outputStream = new FileOutputStream(new File("/upload/files/"+uuid));

			while ((bytesRead = inputStream.read(buffer)) != -1) {
				outputStream.write(buffer, 0, bytesRead); // WHAT: Flushes the buffered data fragment bytes directly onto the server hard drive filesystem partition storage block.
				
				totalBytesRead += bytesRead; // WHAT: Increments the total progress counter with the volume of bytes written during the current iteration step.
				
				// WHAT: Calculates the percentage completion score mathematically by executing: (Current Chunks Processing * 100) divided by Total Expected File Size.
				// WHY: Casts the calculation into a clean integer format to fit a standard progress tracking status payload seamlessly.
                		
				int progress = (int) ((totalBytesRead * 100) / fileSize);
				progressTracker.put(uploadId, progress); 

				// WHAT: Atomically updates or inserts the computed completion percentage index score directly into the ConcurrentHashMap cache structure.
			}
		}

		return ResponseEntity.ok("Upload complete: " + uploadId);
	}


	@GetMapping("/progress/{uploadId}") // WHAT: Defines an HTTP GET routing endpoint path parameterized by a dynamic uploadId path variable segment.
    	public Integer getProgress(@PathVariable String uploadId) 
	{ 	// WHAT: Extracts the tracking variable token parameter out of the active URL request structure path.
		// WHAT: Queries the thread-safe map cache structure using the provided uploadId lookup string.
		// WHY: Safely pulls back the real-time upload progress score; returns a default value of 0 if the query ID doesn't exist in the active cache.
		return progressTracker.getOrDefault(uploadId, 0);
    	}
}