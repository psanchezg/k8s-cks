# Minimize Image Footprint

## Best practices
- Multi-stage images
- Use minimal images
- Pin the exact version: golang:1.23-alpine
- Avoid running application process as the root user.
  + RUN addgroup -S app && adduser -S app -G app -h /home/app
  + USER app:app
  + COPY --from=stage-0 /app /home/app
  + CMD ["/home/app/app"]

