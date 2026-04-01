```java
@RequestMapping("/me")  
ResponseEntity<String> mePage(HttpServletResponse response){  
    var jwtRefreshToken = jwtService.generateToken();  
    var cookie = new Cookie("refreshToken" , jwtRefreshToken);  
    cookie.setHttpOnly(true); // This disable accessing cookie using javascript.  
    cookie.setPath("/api/refresh"); // specify which endpoint this cookie will be sent to.  
    cookie.setMaxAge(604800);  
    cookie.setSecure(true); // https only  
  
    response.addCookie(cookie);  
    return ResponseEntity.ok(email);  
}
```