 # Stage 1: Build the Go binary
 #golang:1.21 provides the Go compiler and necessary tools for building Go applications
 #and assigns the name builder to this stage.
 FROM golang:1.21-alpine AS builder
 
 # Set a non-root user for safety 
 #This command creates a new user named builderuser inside the container.
 #-D: This flag tells adduser to create the user without a password and without creating a home directory,
 #Running build processes as a non-root user is a security best practice.
 RUN adduser -D builderuser
 
 # All subsequent file operations (like COPY or go build) will happen relative to 
 #/app inside the container as we set this path as work directory inside the container
 WORKDIR /app
 
 # Copy only necessary files
 # From Local Copied main.go and index.html inside /app(. represent the current dir) inside the container
 COPY main.go .
 COPY index.html .
 
 #CGO_ENABLED=0 :means the Go application will be statically linked, producing a single executable file .
 #GOOS=linux: This explicitly tells the Go compiler to build an executable for the Linux operating system.
 #go build: The standard Go command to compile a package.
 #-o server: Specifies the output filename for the compiled executable as server.
 #main.go: The input source file to compile. 
 #Compiles your Go application into a standalone binary named server.
 # Build the app statically to remove libc dependency
 RUN CGO_ENABLED=0 GOOS=linux go build -o server main.go
 
 
 #scratch: This is a special, empty base image provided by Docker. It contains absolutely nothing—no 
 #operating system, no shell, no libraries, just an empty filesystem.
 # Stage 2: Minimal secure runtime
 FROM scratch
 
 # Set working directory
 WORKDIR /app
 
 #This is how you copy files from a previous stage
 # Copy binary and html file from builder
 COPY --from=builder /app/server .
 COPY --from=builder /app/index.html .
 
 # Use a non-root user
 #1000: A common numeric user ID for non-root users. 
 USER 1000
 
 # Expose port
 #This is primarily documentation. It tells anyone using the image that the application 
 #inside expects to communicate on port 8080.Explicitly we have to publish the port.
 EXPOSE 8080
 
 # Run the binary
 #It means that the command ./server will be executed directly.
 ENTRYPOINT ["./server"]