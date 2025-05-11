# ---------- Builder Stage ----------
    FROM eclipse-temurin:21-jdk AS builder

    WORKDIR /usr/src/app/
    
    # Copy Gradle wrapper and config files first to leverage caching
    COPY gradlew settings.gradle build.gradle ./
    COPY gradle/ gradle/
    
    # Make Gradle wrapper executable
    RUN chmod +x gradlew
    
    # Download dependencies early to cache them
    RUN ./gradlew downloadRepos
    
    # Copy source code
    COPY . .
    
    # Optional: If .proto files are in ./pb and expected in ./proto
    RUN mkdir -p proto && cp -r pb/* proto/
    
    # Build the project
    RUN ./gradlew installDist -PprotoSourceDir=./proto
    
    # ---------- Runtime Stage ----------
    FROM eclipse-temurin:21-jre
    
    WORKDIR /usr/src/app/
    
    # Copy built artifacts from builder stage
    COPY --from=builder /usr/src/app/ ./
    
    ENV AD_PORT=9099
    
    ENTRYPOINT ["./build/install/opentelemetry-demo-ad/bin/Ad"]
    