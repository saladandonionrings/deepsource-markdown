# Function with cyclomatic complexity higher than threshold found
**ID:** `JAVA-R1000` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-R1000)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A function with high cyclomatic complexity can be hard to understand and maintain. Cyclomatic complexity is a software metric that measures the number of independent paths through a function. A higher cyclomatic complexity indicates that the function has more decision points and is more complex.

Functions with high cyclomatic complexity are more likely to have bugs and be harder to test. They may lead to reduced code maintainability and increased development time.

To reduce the cyclomatic complexity of a function, you can:

* Break the function into smaller, more manageable functions.* Refactor complex logic into separate functions or classes.* Avoid multiple return paths and deeply nested control expressions.

## Bad practice
The method below (from the source code of the Maven build system, non-branch lines have been abbreviated) has a complexity of 25, and should be refactored if possible.


```java
public VersionResult resolveVersion(RepositorySystemSession session, VersionRequest request) // 1
            throws VersionResolutionException {
        // ...

        if (cache != null && !ConfigUtils.getBoolean(session, false, "aether.versionResolver.noCache")) { // +2
            // ...
            if (obj instanceof Record) { // +1

            }
        }

        Metadata metadata = null;

        // This section could be refactored, as all operations here are independent of external control flow.
        if (RELEASE.equals(version)) { // +1
            metadata = new DefaultMetadata(
                    artifact.getGroupId(), artifact.getArtifactId(), MAVEN_METADATA_XML, Metadata.Nature.RELEASE);
        } else if (LATEST.equals(version)) { // +1
            metadata = new DefaultMetadata(
                    artifact.getGroupId(),
                    artifact.getArtifactId(),
                    MAVEN_METADATA_XML,
                    Metadata.Nature.RELEASE_OR_SNAPSHOT);
        } else if (version.endsWith(SNAPSHOT)) { // +1
            WorkspaceReader workspace = session.getWorkspaceReader();
            if (workspace != null && workspace.findVersions(artifact).contains(version)) { // +2
                metadata = null;
                result.setRepository(workspace.getRepository());
            } else {
                metadata = new DefaultMetadata(
                        artifact.getGroupId(),
                        artifact.getArtifactId(),
                        version,
                        MAVEN_METADATA_XML,
                        Metadata.Nature.SNAPSHOT);
            }
        } else {
            metadata = null;
        }

        if (metadata == null) { // +1
            result.setVersion(version);
        } else {
            // ...
            for (RemoteRepository repository : request.getRepositories()) { // +1
                // ...
            }

            // ...

            for (MetadataResult metadataResult : metadataResults) { // +1
                // ...
                if (repository == null) { // +1
                    // ...
                }

                Versioning v = readVersions(session, trace, metadataResult.getMetadata(), repository, result);
                merge(artifact, infos, v, repository);
            }

            // This section could also be extracted for the same reasons.
            if (RELEASE.equals(version)) { // +1
                resolve(result, infos, RELEASE);
            } else if (LATEST.equals(version)) { // +1
                if (!resolve(result, infos, LATEST)) { // +1
                    resolve(result, infos, RELEASE);
                }

                if (result.getVersion() != null && result.getVersion().endsWith(SNAPSHOT)) { // +2
                    VersionRequest subRequest = new VersionRequest();
                    subRequest.setArtifact(artifact.setVersion(result.getVersion()));
                    if (result.getRepository() instanceof RemoteRepository) { // +1
                        RemoteRepository r = (RemoteRepository) result.getRepository();
                        subRequest.setRepositories(Collections.singletonList(r));
                    } else {
                        subRequest.setRepositories(request.getRepositories());
                    }
                    VersionResult subResult = resolveVersion(session, subRequest);
                    result.setVersion(subResult.getVersion());
                    result.setRepository(subResult.getRepository());
                    for (Exception exception : subResult.getExceptions()) { // +1
                        result.addException(exception);
                    }
                }
            } else {
                String key = SNAPSHOT + getKey(artifact.getClassifier(), artifact.getExtension());
                merge(infos, SNAPSHOT, key);
                if (!resolve(result, infos, key)) { // +1
                    result.setVersion(version);
                }
            }

            if (StringUtils.isEmpty(result.getVersion())) { // +1
                throw new VersionResolutionException(result);
            }
        }

        if (cacheKey != null && metadata != null && isSafelyCacheable(session, artifact)) { // +3
            cache.put(session, cacheKey, new Record(result.getVersion(), result.getRepository()));
        }

        return result;
    }
```

## Recommended
It is best to refactor the method into multiple separate methods, so that the complexity of individual methods is reduced.

Here, after extracting the parts of the code highlighted above, the complexity is reduced to `12`, and shifted into two other methods instead.


```java
public VersionResult resolveVersion(RepositorySystemSession session, VersionRequest request) // 1
        throws VersionResolutionException {
    // ...

    if (cache != null && !ConfigUtils.getBoolean(session, false, "aether.versionResolver.noCache")) { // +2
        // ...
        if (obj instanceof Record) { // +1
            // ...
        }
    }

    Metadata metadata = getMetadataForVersion(session, version, artifact, result);

    if (metadata == null) { // +1
        // ...
    } else {
        // ...
        for (RemoteRepository repository : request.getRepositories()) { // +1
            // ...
        }

        // ...

        for (MetadataResult metadataResult : metadataResults) { // +1
            // ...
            if (repository == null) { // +1
                // ...
            }
            // ...
        }

        resolveBasedOnVersion(session, request, version, result, infos, artifact);

        if (StringUtils.isEmpty(result.getVersion())) { // +1
            throw new VersionResolutionException(result);
        }
    }

    if (cacheKey != null && metadata != null && isSafelyCacheable(session, artifact)) { // +3
        cache.put(session, cacheKey, new Record(result.getVersion(), result.getRepository()));
    }

    return result;
}

private void resolveBasedOnVersion(RepositorySystemSession session, VersionRequest request, String version, VersionResult result, Map<String, VersionInfo> infos, Artifact artifact) throws VersionResolutionException {
    if (RELEASE.equals(version)) { // +1
        resolve(result, infos, RELEASE);
    } else if (LATEST.equals(version)) { // +1
        if (!resolve(result, infos, LATEST)) { // +1
            resolve(result, infos, RELEASE);
        }

        if (result.getVersion() != null && result.getVersion().endsWith(SNAPSHOT)) { // +2
            // ...
            if (result.getRepository() instanceof RemoteRepository) { // +1
                // ...
            } else {
                // ...
            }
            // ...
            for (Exception exception : subResult.getExceptions()) { // +1
                result.addException(exception);
            }
        }
    } else {
        // ...
        if (!resolve(result, infos, key)) { // +1
            result.setVersion(version);
        }
    }
}

@Nullable
private static Metadata getMetadataForVersion(RepositorySystemSession session, String version, Artifact artifact, VersionResult result) {
    if (RELEASE.equals(version)) { // +1
        // ...
    } else if (LATEST.equals(version)) { // +1
        // ...
    } else if (version.endsWith(SNAPSHOT)) { // +1
        WorkspaceReader workspace = session.getWorkspaceReader();
        if (workspace != null && workspace.findVersions(artifact).contains(version)) { // +2
            // ...
        } else {
            // ...
        }
    } else {
        metadata = null;
    }
    return metadata;
}
```

## Issue configuration
Cyclomatic complexity threshold can be configured using the `cyclomatic_complexity_threshold` [meta field](https://docs.deepsource.com/docs/analyzers-java#cyclomatic_complexity_threshold) in your repository's `.deepsource.toml` config file.

Configuring this is optional. If you don't provide a value, the Analyzer will raise issues for functions with complexity higher than the default threshold, which is "medium" (which raises issues for complexity > `15`) for the Java Analyzer.

Here's a mapping of risk category to cyclomatic complexity score to help you configure this better:

