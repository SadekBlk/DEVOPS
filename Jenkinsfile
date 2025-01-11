stages:
  - build

variables:
  APP_NAME: anime
  COMMIT: $CI_COMMIT_SHORT_SHA
  REGISTRY_USERNAME: your-dockerhub-username  # Replace with your Docker Hub username
  REGISTRY_PASSWORD: $DOCKERHUB_PASSWORD      # Store this as a CI/CD variable in GitLab

build_app:
  stage: build
  only:
    changes:
      - "src/**/*"
  image: docker:latest
  services:
    - docker:dind
  before_script:
    - echo "Logging into Docker Hub..."
    - echo "$REGISTRY_PASSWORD" | docker login -u "$REGISTRY_USERNAME" --password-stdin
  script:
    - echo "Building Docker image..."
    - docker build -t "$REGISTRY_USERNAME/$APP_NAME:$COMMIT" .
    - echo "Pushing Docker image to Docker Hub..."
    - docker push "$REGISTRY_USERNAME/$APP_NAME:$COMMIT"
    - echo "Cleaning up Docker image..."
    - docker rmi "$REGISTRY_USERNAME/$APP_NAME:$COMMIT"  # Remove the image from the pipeline server
    - docker logout "$REGISTRY_USERNAME"
  after_script:
    - echo "Build stage completed."
  rules:
    - if: $CI_COMMIT_BRANCH == "main"  # Only run this job on the main branch