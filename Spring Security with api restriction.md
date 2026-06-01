Step 1: Create the Custom Security Evaluator
This component will house your custom verification logic. It checks if the logged-in user matches the ID of the profile they are trying to update.

Java
package com.example.demo.security;

import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.stereotype.Component;

@Component("profileSecurity")
public class ProfileSecurityService {

    public boolean isProfileOwner(String profileId) {
        // 1. Fetch the current logged-in user's authentication data
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        
        if (authentication == null || !authentication.isAuthenticated()) {
            return false;
        }

        // 2. Extract the username/email
        String loggedInUsername = authentication.getName();

        // 3. Custom logic: Only allow access if the logged-in username matches the profileId
        return loggedInUsername.equalsIgnoreCase(profileId);
    }
}
Step 2: Implement the Controller with 3 APIs
Here is the single controller showing all three access levels side-by-side:

Java
package com.example.demo.controller;

import org.springframework.http.ResponseEntity;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/profiles")
public class ProfileController {

    // Tier 1: COMPLETELY FREE API (No annotation needed, allowed globally)
    @GetMapping("/faq")
    public ResponseEntity<String> getFAQ() {
        return ResponseEntity.ok("Public FAQ Content: Anyone can read this without logging in.");
    }

    // Tier 2: RESTRICTED VIA BUILT-IN METHOD (Any logged-in user can access)
    @GetMapping("/dashboard")
    @PreAuthorize("isAuthenticated()") 
    public ResponseEntity<String> getGeneralDashboard() {
        return ResponseEntity.ok("Secured Content: Welcome! You see this because you are logged in.");
    }

    // Tier 3: RESTRICTED VIA CUSTOM METHOD (Strict custom ownership validation)
    @PutMapping("/{profileId}/update")
    @PreAuthorize("@profileSecurity.isProfileOwner(#profileId)")
    public ResponseEntity<String> updateProfile(@PathVariable String profileId, @RequestBody String updateData) {
        return ResponseEntity.ok("Custom Secured Content: Profile for '" + profileId + "' has been successfully updated.");
    }
}
Step 3: Global Spring Security Configuration
To make sure Spring Security triggers the annotations correctly, configure your security filter chain to hand off authorization control to the method layer.

Java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity // Essential to activate @PreAuthorize
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable()) // Disabled for stateless REST testing
            .authorizeHttpRequests(auth -> auth
                // Allow requests to reach the controller layer unimpeded 
                // so the method-level annotations can do their job
                .anyRequest().permitAll() 
            )
            // Enabling basic authentication for testing (Username/Password)
            .httpBasic(Customizer.withDefaults()); 

        return http.build();
    }
}
How to Test This Setup
Test API 1 (Free): Send a GET request to http://localhost:8080/api/profiles/faq. It will return 200 OK instantly without asking for credentials.

Test API 2 (Authenticated): Send a GET request to http://localhost:8080/api/profiles/dashboard. If you do not pass a login header, it will return 403 Forbidden. If you log in with any valid account, it returns 200 OK.

Test API 3 (Custom Logic): Log in as a user named john_doe and send a PUT request to http://localhost:8080/api/profiles/john_doe/update. It returns 200 OK. If john_doe tries to execute a PUT request against http://localhost:8080/api/profiles/alex_smith/update, your custom component evaluates it to false and returns 403 Forbidden.