# Briisk git Release

## Table of Contents

  1. [First release](#first-release)
  1. [Bugfix to release](#bugfix-to-release)
  1. [Hotfix to production release](#hotfix-to-production-release)

## First Release

1. Bump version in these files with complance to [Semantic Versioning](https://semver.org)
  - in `package.json`  for Nodejs/Web app
  - in `mix.exs` for Elixir app
  - in `gemfile` for Rails app
  - in `ios/ProjectName/Info.plist` for iOS apps
  - in `android/app/build.gradle` for Android apps

2. Commit changes with: `git commit -m "version 0.{release_number}.0" --no-verify`

3. Create a tag with: `git tag -a v0.{release_number}.0 -m "version v0.{release_number}.0"`

4. Push changes and tags with: `git push && git push --tags`

5. Create release branch with: `git checkout -b version-0.{release_number}.x`

6. Push release branch with: `git push -u origin version-0.{release_number}.x`

7. Change branch on CI/CD (currently SemphoreCI) for staging server to: `version-0.{release_number}.x`

8. Trigger manual deploy for staging environment on CI/CD

9. Label all user stories concerning particular release with: `API-0.{release_number}.x`, `Front-0.{release_number}.x`, `Mobile-0.{release_number}.x`


**[⬆ back to top](#table-of-contents)**

## Bugfix to Release

1. Make changes on a new branch created from `version-0.{release_number}.x` branch

2. Commit them, make Pull Request, after Code Review Merge changes

3. On branch `version-0.{release_number}.x` bump version in these files (last number), e.g.: branch `version-0.2.x`, new version `0.2.1`
  - in `package.json`  for Nodejs/Web app
  - in `mix.exs` for Elixir app
  - in `gemfile` for Rails app
  - in `ios/ProjectName/Info.plist` for iOS apps
  - in `android/app/build.gradle` for Android apps

2. Commit changes with: `git commit -m "version 0.{release_number}.{bugfix_number}" --no-verify`

3. Create a tag with: `git tag -a v0.{release_number}.{bugfix_number} -m "version v0.{release_number}.{bugfix_number}"`

4. Push changes and tags with: `git push && git push --tags`

5. Sync `version-0.{release_number}.x` branch with `workspace` (create PR  `version-0.{release_number}.x` => `workspace`)

6. Label bug on JIRA concerning particular release with: `API-0.{release_number}.x`, `Front-0.{release_number}.x`, `Mobile-0.{release_number}.x`

**[⬆ back to top](#table-of-contents)**


## Hotfix to Production Release

1. Make changes on `version-0.{release_number}.x` branch

2. Commit them

3. Bump version in these files (last number), e.g.: branch `version-0.2.x`, new version `0.2.1`
  - in `package.json`  for Nodejs/Web app
  - in `mix.exs` for Elixir app
  - in `gemfile` for Rails app
  - in `ios/ProjectName/Info.plist` for iOS apps
  - in `android/app/build.gradle` for Android apps

2. Commit changes with: `git commit -m "version 0.{release_number}.{bugfix_number}" --no-verify`

3. Create a tag with: `git tag -a v0.{release_number}.{bugfix_number} -m "version v0.{release_number}.{bugfix_number}"`

4. Push changes and tags with: `git push && git push --tags`

5. Sync `version-0.{release_number}.x` branch with `workspace` (create PR  `version-0.{release_number}.x` => `workspace`)

6. Label bug on JIRA concerning particular release with: `API-0.{release_number}.x`, `Front-0.{release_number}.x`, `Mobile-0.{release_number}.x`

**[⬆ back to top](#table-of-contents)**

