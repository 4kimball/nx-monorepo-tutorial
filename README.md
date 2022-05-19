# nx-monorepo-tutorial

> nx를 활용하여 모노레포를 구성하기 위한 준비
> nx는 모노레포 구성을 도와줄 뿐만 아니라 변경된 코드에 대한 의존성 확인, 의존성 시각화, e2e 테스트 등 많은 것을 제공해준다.

### Next App을 포함한 모노레포 만들기

- 아래의 명령을 통해 `app/`에 Next App을 생성한 모노레포를 생성할 수 있다.

```shell
npx create-nx-workspace --preset=next
```

- `preset`에는 next뿐만 아니라 react도 가능하다.
- 위의 명령을 실행하면 workspace의 이름, next app의 이름을 입력 한후에 nx cloud의 사용여부를 물어본다.

### 대표적인 디렉토리 파악하기

- `apps/` : 하나의 애플리케이션 단위의 프로젝트들이 존재한다.
- `libs/` : 전반적인 애플리케이션에서 공통적으로 사용될 코드를 작성한다.
- `tools/` : 개발에 필요한 tooling script가 존재한다.

### 대표적인 파일 파악하기

nx로 모노레포를 구성하면 처음보는 파일들이 보인다. 이 파일들의 역할을 알아야 할 필요가 있다.

- nx.json : Nx CLI 및 프로젝트 기본값들이 정의되어 있다.
- workspace.json : Nx가 root의 package.json의 작업 공간 속성에 지정된 glob과 일치하는 모든 proeject.json 및 package.json에 대한 파일 트리를 스캔하는 대신 이 파일을 통해 명시적으로 나열할 수 있다.
- project.json : 이 파일은 Nx Plugins를 사용할 때 생성된다. npm 스크립트 대신 사용되는 사용자 지정 generate를 구성한다. 예를 들어 아래와 같을 수 있다. 해당 프로젝트에 `build` 명령을 실행하면 Nx Plugins 중의 하나인 `nrwl/next:build`를 실행할 수 있다.

```json
 "targets": {
    "build": {
      "executor": "@nrwl/next:build",
      "outputs": ["{options.outputPath}"],
      "defaultConfiguration": "production",
      "options": {
        "root": "apps/next-app",
        "outputPath": "dist/apps/next-app"
      },
      "configurations": {
        "development": {},
        "production": {}
      }
    }
 }
```

### 라이브러리 추가하기

Nx Plugins를 활용하여 apps에서 공통적으로 사용할 react-ui를 추가할 수 있다.

```shell
npx nx g @nrwl/react:lib react-ui
```

위의 명령을 실행하면 리액트 기반의 ui를 생성할 수 있는 준비가 된다.
nx가 global하게 설치되었다면 npx를 생략할 수 있고, `g`는 `generate`의 약자이다.

이후 아래와 같이 Header 컴포넌트를 작성하였다.

```typescript
import React, { FC } from 'react';

interface Props {
  title: string;
}

export const Header: FC<Props> = ({ title }) => {
  return (
    <>
      <h1>{title}</h1>
    </>
  );
};
```

### 라이브러리 사용하기

위에서 생성한 react-ui의 Header 컴포넌트를 사용하기 위해서는 아래와 같이 추가할 수 있다.

```javascript
import { Header } from '@nx-monorepo-tutorial/react-ui';
```

### 생성한 Next App 실행하기

- `app/next-app`의 project.json에는 `serve` 로 next app을 실행할 수 있는 명령이 포함되어 있다.
- 아래의 명령어를 실행하면 해당 next app이 실행된다.

```shell
npx nx serve next-app
```
