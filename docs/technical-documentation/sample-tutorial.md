# SAMPLE - Building a product creation POST endpoint in Spring Boot

!!! info "About this sample"
    This tutorial explains how to implement the “Add Product” endpoint in a fictional e-commerce API. It is aimed at backend developers who must integrate new product creation functionality into their services.

    Some specific blocks and snippets are tailored specifically for junior developers who may need extra context.

    The guide follows a codelab-style format and assumes a starter project that readers progress from an initial state to a final solution.

## 1. Overview

In this tutorial, **you will learn how to extend an existing Spring Boot REST API by adding a `POST` endpoint** that lets clients create new products. This is a common step in API development—moving from read-only endpoints to full CRUD support. You will work with several core concepts, including request modeling, validation, controller design, service-layer logic, and automated testing.

You will work with a starter project that already exposes `GET /products` and `GET /products/{id}` endpoints. The application can read product data but cannot create new entries. In this tutorial, **you will close that gap by introducing a structured request object, adding a new method, and connecting it to the service and storage layers**. You will also add targeted tests to verify correct behavior for both valid and invalid input.

The goal is not only to make the endpoint work, but **to implement it in a way that is idiomatic to Spring Boot and maintainable over time**. You will use validation annotations, clear HTTP status codes, and a clean separation of concerns between layers.

By the end of this tutorial, **you will have a fully functional `POST /products` endpoint** that accepts JSON, validates input automatically, stores a new product, and returns a proper REST response.

!!! tip "About the tips in this tutorial"
    Throughout the tutorial, you’ll see highlighted tips like this. These provide additional explanations for concepts that may be new or unfamiliar.

    **If you’re a junior developer**, these tips can be helpful in your learning journey.
    **If you’re a senior developer**, feel free to skip them.

### What you’ll learn

- How to add a `POST` endpoint to a Spring Boot REST API.
- How to restrict and validate the values sent to the `POST` endpoint.
- How to create unit tests to ensure the `POST` endpoint works as intended.
- How to design and use a DTO (Data Transfer Object) for incoming HTTP requests.

### What you’ll need

- Basic familiarity with Java and Spring Boot
- Comfort reading and editing Java classes
- A development environment capable of running Spring Boot applications
- The starter project provided for this tutorial

### Prerequisites

- Java 21.0.10
- Apache Maven 3.9.12
- An IDE such as VS Code, IntelliJ IDEA, or Eclipse

## 2. Define accepted fields

### 2.1 Objective

**Create a dedicated request object** that represents the JSON body clients will send when creating a product, and **enforce validation rules** so invalid data is rejected automatically before it reaches your business logic.

Instead of accepting a raw `Product` entity in the controller, **you will introduce a DTO (Data Transfer Object) called `CreateProductRequest`**. This keeps your API contract focused only on the fields required for creation and prevents accidental exposure or misuse of internal model properties (such as IDs).

### 2.2 Process

**Create a folder named `dto`** under `⁠src/main/java/com/example/tutorial/` and a file named `CreateProductRequest.java` inside.

In this file, create the class `CreateProductRequest` inside the ⁠`com.example.tutorial`⁠ package:

/1. **Set the package** we will use for DTO classes:

```java
package com.example.tutorial.dto;

/**
 * Data Transfer Object for creating a new product.
 * Contains validation constraints for input data.
 */
public class CreateProductRequest {
     // We will add the product properties here.
}
```

/2. **Import Lombok annotations** to reduce boilerplate code and add Lombok class-level annotations:

```java
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
```

!!! tip "Common Lombok annotations"
    - `@Data` – Automatically creates common methods like getters, setters, and `toString` so you don’t have to write them.
    - `@Builder` – Lets you create an object step by step in a clear, readable way.
    - `@NoArgsConstructor` – Adds an empty constructor (no inputs).
    - `@AllArgsConstructor` – Adds a constructor that takes every field as an input.

/3. Inside the class, **declare the product properties**:

```java
public class CreateProductRequest {
    private String name;
    private String description;
    private Double price;
    private String category;
    private Integer stock;
} ⁠
```

/4. **Add Jakarta Validation annotations** to express constraints in the product properties:

4.1 **Add the following imports** at the top beneath the package:

```java
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Positive;
import jakarta.validation.constraints.PositiveOrZero;
import jakarta.validation.constraints.Size;
```

4.2 **Add these validation rules** above their corresponding fields:

```java
    @NotBlank(message = "Product name is required")
    @Size(min = 1, max = 100, message = "Product name must be between 1 and 100 characters")
    private String name;

    @Size(max = 500, message = "Description cannot exceed 500 characters")
    private String description;

    @NotNull(message = "Price is required")
    @Positive(message = "Price must be a positive value")
    private Double price;

    @Size(max = 50, message = "Category cannot exceed 50 characters")
    private String category;

    @PositiveOrZero(message = "Stock cannot be negative")
    private Integer stock;
```

!!! tip "Common validation annotations"
    - `@NotBlank`: Value must be a non-empty string containing at least one non-whitespace character.
    - `@Size`: Value length or collection size must fall within the defined min/max limits.
    - `@NotNull`: Value cannot be null.
    - `@Positive`: Numeric value must be greater than 0.
    - `@PositiveOrZero`: Numeric value must be 0 or greater (not negative).

### 2.3 Expected outcome

After completing this step:

- There is a new `CreateProductRequest` class under the `dto` package.
- Validation annotations are in place.
- You now have a clear, safe structure for incoming product-creation data.

By the end of this step, `CreateProductRequest.java` should look like this:

<details>
  <summary>Click to see the code</summary>

```java
package com.example.tutorial.dto;

import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Positive;
import jakarta.validation.constraints.PositiveOrZero;
import jakarta.validation.constraints.Size;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * Data Transfer Object for creating a new product.
 * Contains validation constraints for input data.
 */
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class CreateProductRequest {

    @NotBlank(message = "Product name is required")
    @Size(min = 1, max = 100, message = "Product name must be between 1 and 100 characters")
    private String name;

    @Size(max = 500, message = "Description cannot exceed 500 characters")
    private String description;

    @NotNull(message = "Price is required")
    @Positive(message = "Price must be a positive value")
    private Double price;

    @Size(max = 50, message = "Category cannot exceed 50 characters")
    private String category;

    @PositiveOrZero(message = "Stock cannot be negative")
    private Integer stock;

}
```

</details>

## 3. Implement the service method

### 3.1 Objective

**Implement the service logic that actually creates and stores a product**. This method will be called when the `POST` endpoint receives a valid request. It will convert the incoming `CreateProductRequest` into a `Product` entity and save it through the repository.

### 3.2 Process

/1. Open `src/main/java/com/example/tutorial/service/ProductService.java` and add the DTO import at the top of the file:

```java
import com.example.tutorial.dto.CreateProductRequest;
```

/2. Add the `createProduct` method inside the `ProductService` class:

```java
    /**
     * Creates a new product from the request data.
     *
     * @param request the product creation request
     * @return the created product with generated ID
     */
    public Product createProduct(CreateProductRequest request) {
        Product product = Product.builder()
                .name(request.getName())
                .description(request.getDescription())
                .price(request.getPrice())
                .category(request.getCategory())
                .stock(request.getStock() != null ? request.getStock() : 0)
                .build();

        return productRepository.save(product);
    }
```

This method:

- Builds a new `Product` using the values from the request DTO so you do not have to set the `id` manually — the repository layer is responsible for generating it.
- If the request omits `stock` (`null`), defaults it to `0` before saving.
- Stores and returns the entity using `productRepository.save(product)`.

### 3.3 Expected outcome

After completing this step:

- `ProductService` contains a working `createProduct` method.

By the end of this step, `ProductService.java` should look like this:

<details>
  <summary>Click to see the code</summary>

```java
package com.example.tutorial.service;

import com.example.tutorial.dto.CreateProductRequest;
import com.example.tutorial.model.Product;
import com.example.tutorial.repository.ProductRepository;

import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

/**
 * Service layer for Product operations.
 */
@Service
public class ProductService {

    private final ProductRepository productRepository;

    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    /**
     * Retrieves all products.
     *
     * @return list of all products
     */
    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    /**
     * Finds a product by its ID.
     *
     * @param id the product ID
     * @return an Optional containing the product if found
     */
    public Optional<Product> getProductById(String id) {
        return productRepository.findById(id);
    }

    /**
     * Creates a new product from the request data.
     *
     * @param request the product creation request
     * @return the created product with generated ID
     */
    public Product createProduct(CreateProductRequest request) {
        Product product = Product.builder()
                .name(request.getName())
                .description(request.getDescription())
                .price(request.getPrice())
                .category(request.getCategory())
                .stock(request.getStock() != null ? request.getStock() : 0)
                .build();

        return productRepository.save(product);
    }

}
```

</details>

## 4. Create the POST /products endpoint and add tests

### 4.1 Objective

**Transform your API from read-only to write-enabled** by implementing the endpoint that allows clients to create new products.

You will add a new method in `ProductController` that handles `POST /products` and does the following, in order:

1. Receives JSON from the request body and maps it to `CreateProductRequest`.
2. Validates that request using the DTO constraints (via `@Valid`).
3. Calls the service layer (`productService.createProduct(...)`) to create and store the product.
4. Returns a `201 Created` response with the created `Product` in the response body.

You will also add focused tests that confirm the API returns the expected HTTP status codes and response structure, ensuring the new `POST` endpoint is both functional and reliably validated through automated testing.

### 4.2 Process

/1. Open `⁠src/main/java/com/example/tutorial/controller/ProductController.java` and **add the required imports** at the top of the file:

```java
    import com.example.tutorial.dto.CreateProductRequest;

    import jakarta.validation.Valid;

    import org.springframework.http.HttpStatus;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestBody;
```

!!! tip "Controller annotations explained"
    - `CreateProductRequest` is the type Spring will deserialize incoming JSON into.
    - `@Valid` tells Spring to run validation rules defined on the DTO.
    - `@PostMapping` maps the method to HTTP POST requests. When a request is made to `http://localhost:8080/products` using the `POST` method, it will execute the contents of `createProduct(...)`.
    - `@RequestBody` binds the request JSON payload to the DTO parameter.
    - `HttpStatus` provides the status code that our API endpoint will return on success.

/2. **Replace `// TODO` with the `createProduct` endpoint method**:

```java
/**
 * Creates a new product.
 *
 * @param request the product creation request
 * @return the created product with HTTP 201 status
 */
@PostMapping
@Operation(summary = "Create a new product", description = "Creates a new product in the catalog")
@ApiResponses(value = {
        @ApiResponse(responseCode = "201", description = "Product created successfully"),
        @ApiResponse(responseCode = "400", description = "Invalid request data")
})
public ResponseEntity<Product> createProduct(
        @Parameter(description = "Product creation data") @Valid @RequestBody CreateProductRequest request) {
    Product createdProduct = productService.createProduct(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(createdProduct);
}
```

!!! tip "How the POST controller works"
    **`@PostMapping` and `public ResponseEntity<Product> createProduct(...)` define the `POST` endpoint**  
    `@PostMapping` maps the method to `POST /products`, while `ResponseEntity<Product>` lets you control both the HTTP status code and the response body returned to the client.

    **`@RequestBody CreateProductRequest request` and `@Valid` handle input and validation.**
    `@RequestBody` converts incoming JSON into the `CreateProductRequest` DTO, which contains only the fields needed for creation.  
    `@Valid` activates the validation rules defined in the DTO. If the data is invalid, Spring automatically returns `400 Bad Request` and the method does not execute.

    **`productService.createProduct(request)` and `ResponseEntity.status(HttpStatus.CREATED).body(createdProduct)` handle business delegation and the HTTP response.**  
    The controller delegates creation logic to the service layer so it stays focused on HTTP concerns, while the service manages business and storage logic.  
    `HttpStatus.CREATED (201)` indicates a new resource was successfully created, and `.body(createdProduct)` returns the newly created `Product` to the client.

    **The Swagger / OpenAPI annotations (`@Operation`, `@ApiResponses`) generate API documentation and describe the endpoint’s behavior, expected inputs, and possible responses.**  
    This will be available at `http://localhost:8080/swagger-ui.html`.

After this, add controller tests for `POST /products`:

/1. Open `src/test/java/com/example/tutorial/controller/ProductControllerTest.java` and add the required imports at the top of the file:

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.example.tutorial.dto.CreateProductRequest;

import static org.mockito.ArgumentMatchers.any;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
```

- `ObjectMapper` converts Java objects to JSON.
- `CreateProductRequest` is the request DTO used to build the test input object.
- `ArgumentMatchers.any` matches any value of a given type when mocking method calls.
- `post` (`MockMvcRequestBuilders`) builds HTTP `POST` requests in tests.

/2. Inside the test class, add `ObjectMapper`:

```java
@Autowired
private ObjectMapper objectMapper;
```

/3. Add the “valid request returns 201” test method:

```java
    @Test
    @DisplayName("POST /products should create and return new product")
    void createProduct_WithValidData_ShouldReturn201() throws Exception {
        CreateProductRequest request = CreateProductRequest.builder()
                .name("New Product")
                .description("A new product")
                .price(39.99)
                .category("New")
                .stock(5)
                .build();

        Product createdProduct = Product.builder()
                .id("new-id-456")
                .name("New Product")
                .description("A new product")
                .price(39.99)
                .category("New")
                .stock(5)
                .build();

        when(productService.createProduct(any(CreateProductRequest.class))).thenReturn(createdProduct);

        mockMvc.perform(post("/products")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(request)))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.id").value("new-id-456"))
                .andExpect(jsonPath("$.name").value("New Product"))
                .andExpect(jsonPath("$.price").value(39.99));
    }
```

!!! tip "What this test verifies"
    This test checks that the controller correctly handles a JSON request, returns a `201 Created` response with the expected JSON fields, and uses a mocked service so only the controller’s HTTP behavior—not the service logic—is being verified.

/4. Add the “invalid request returns 400” test method:

```java
    @Test
    @DisplayName("POST /products should return 400 for invalid data")
    void createProduct_WithInvalidData_ShouldReturn400() throws Exception {
        CreateProductRequest invalidRequest = CreateProductRequest.builder()
                .name("")  // Invalid: blank name
                .price(-10.0)  // Invalid: negative price
                .build();

        mockMvc.perform(post("/products")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(invalidRequest)))
                .andExpect(status().isBadRequest());
    }
```

!!! tip "Validation test purpose"
    This test verifies that request validation is enforced by the controller, returning a `400 Bad Request` for invalid input without needing to mock the service since validation occurs before the service is invoked.

### 4.3 Expected outcome

After completing this step:

- Your controller compiles with a new `createProduct(...)` method.
- `POST /products` is fully functional and accepts JSON.
- Valid requests will result in:
  - `201 Created`.
  - A JSON `Product` response.
- Invalid requests will result in:
  - `400 Bad Request` automatically due to `@Valid`.
- Your test suite covers the new endpoint’s success and failure paths, helping prevent regressions later.

By the end of this step, `ProductController.java` and `ProductControllerTest.java` files should look like this:

- `ProductController.java`

<details>
  <summary>Click to see the code</summary>

```java
package com.example.tutorial.controller;

import com.example.tutorial.dto.CreateProductRequest;
import com.example.tutorial.model.Product;
import com.example.tutorial.service.ProductService;

import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.responses.ApiResponses;
import io.swagger.v3.oas.annotations.tags.Tag;

import jakarta.validation.Valid;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

/**
 * REST Controller for Product operations.
 */
@RestController
@RequestMapping("/products")
@Tag(name = "Products", description = "Product management endpoints")
public class ProductController {

    private final ProductService productService;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    /**
     * Retrieves all products.
     *
     * @return list of all products
     */
    @GetMapping
    @Operation(summary = "Get all products", description = "Retrieves a list of all available products")
    @ApiResponse(responseCode = "200", description = "Successfully retrieved products")
    public ResponseEntity<List<Product>> getAllProducts() {
        List<Product> products = productService.getAllProducts();
        return ResponseEntity.ok(products);
    }

    /**
     * Retrieves a product by its ID.
     *
     * @param id the product ID
     * @return the product if found, or 404 if not found
     */
    @GetMapping("/{id}")
    @Operation(summary = "Get product by ID", description = "Retrieves a specific product by its unique identifier")
    @ApiResponses(value = {
            @ApiResponse(responseCode = "200", description = "Product found"),
            @ApiResponse(responseCode = "404", description = "Product not found")
    })
    public ResponseEntity<Product> getProductById(
            @Parameter(description = "Product ID") @PathVariable String id) {
        return productService.getProductById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    /**
     * Creates a new product.
     *
     * @param request the product creation request
     * @return the created product with HTTP 201 status
     */
    @PostMapping
    @Operation(summary = "Create a new product", description = "Creates a new product in the catalog")
    @ApiResponses(value = {
            @ApiResponse(responseCode = "201", description = "Product created successfully"),
            @ApiResponse(responseCode = "400", description = "Invalid request data")
    })
    public ResponseEntity<Product> createProduct(
            @Parameter(description = "Product creation data") @Valid @RequestBody CreateProductRequest request) {
        Product createdProduct = productService.createProduct(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdProduct);
    }

}
```

</details>

- `ProductControllerTest.java`

<details>
  <summary>Click to see the code</summary>

```java
package com.example.tutorial.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.example.tutorial.dto.CreateProductRequest;
import com.example.tutorial.model.Product;
import com.example.tutorial.service.ProductService;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import java.util.Arrays;
import java.util.Optional;

import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(ProductController.class)
class ProductControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private ObjectMapper objectMapper;

    @MockBean
    private ProductService productService;

    private Product testProduct;

    @BeforeEach
    void setUp() {
        testProduct = Product.builder()
                .id("test-id-123")
                .name("Test Product")
                .description("A test product")
                .price(29.99)
                .category("Test")
                .stock(10)
                .build();
    }

    @Test
    @DisplayName("GET /products should return all products")
    void getAllProducts_ShouldReturnProductList() throws Exception {
        when(productService.getAllProducts()).thenReturn(Arrays.asList(testProduct));

        mockMvc.perform(get("/products")
                        .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].name").value("Test Product"))
                .andExpect(jsonPath("$[0].price").value(29.99));
    }

    @Test
    @DisplayName("GET /products/{id} should return product when found")
    void getProductById_WhenFound_ShouldReturnProduct() throws Exception {
        when(productService.getProductById("test-id-123")).thenReturn(Optional.of(testProduct));

        mockMvc.perform(get("/products/test-id-123")
                        .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Test Product"))
                .andExpect(jsonPath("$.id").value("test-id-123"));
    }

    @Test
    @DisplayName("GET /products/{id} should return 404 when not found")
    void getProductById_WhenNotFound_ShouldReturn404() throws Exception {
        when(productService.getProductById("non-existent")).thenReturn(Optional.empty());

        mockMvc.perform(get("/products/non-existent")
                        .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound());
    }

    @Test
    @DisplayName("POST /products should create and return new product")
    void createProduct_WithValidData_ShouldReturn201() throws Exception {
        CreateProductRequest request = CreateProductRequest.builder()
                .name("New Product")
                .description("A new product")
                .price(39.99)
                .category("New")
                .stock(5)
                .build();

        Product createdProduct = Product.builder()
                .id("new-id-456")
                .name("New Product")
                .description("A new product")
                .price(39.99)
                .category("New")
                .stock(5)
                .build();

        when(productService.createProduct(any(CreateProductRequest.class))).thenReturn(createdProduct);

        mockMvc.perform(post("/products")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(request)))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.id").value("new-id-456"))
                .andExpect(jsonPath("$.name").value("New Product"))
                .andExpect(jsonPath("$.price").value(39.99));
    }

    @Test
    @DisplayName("POST /products should return 400 for invalid data")
    void createProduct_WithInvalidData_ShouldReturn400() throws Exception {
        CreateProductRequest invalidRequest = CreateProductRequest.builder()
                .name("")  // Invalid: blank name
                .price(-10.0)  // Invalid: negative price
                .build();

        mockMvc.perform(post("/products")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(invalidRequest)))
                .andExpect(status().isBadRequest());
    }
}
```

</details>

## 5. Summary

!!! success "Congratulations!
    In this tutorial, you have added a `POST` endpoint to create new products in a Spring Boot application. You learned how to update the controller (`ProductController`) with a new `@PostMapping` method that handles JSON input and produces a `201 Created` response. You also implemented the corresponding service logic in `ProductService` to build and save a `Product` from a `CreateProductRequest` DTO. Additionally, you saw how validation annotations in the DTO automatically enforce rules, returning a `400 Bad Request` for invalid data. Finally, you verified the functionality with unit tests (and optionally with a real HTTP request).
    
    With this new endpoint in place, the API now supports Create operations (in addition to the existing Read operations), moving closer to full CRUD capabilities. You can apply these same patterns to add other endpoints (for updating or deleting products) and be confident in writing clean, well-structured Spring Boot controllers and services.
