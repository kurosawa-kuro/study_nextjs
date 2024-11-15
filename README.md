# study_nextjs

```
npx create-next-app@latest my-app --typescript

npx create-next-app@latest
```

```
・typescript
・tailwind
・react icon
・zustand
・react testing library
・storybook
・react-modal
・Zod
・Formik
・react-hot-toast
・Framer Motion
```

```
# 1. React Icons
npm install react-icons

# 2. Zustand
npm install zustand

# 3. React Testing Library
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event jest jest-environment-jsdom

# 4. Storybook
npx storybook@latest init

# 5. React Modal
npm install react-modal
npm install --save-dev @types/react-modal  # TypeScript型定義

# 6. Zod
npm install zod

# 7. Formik
npm install formik

# 8. React Hot Toast
npm install react-hot-toast

# 9. Framer Motion
npm install framer-motion
```

```
npm install --save-dev jest @testing-library/react @testing-library/jest-dom @testing-library/user-event jest-environment-jsdom
```

```
// jest.config.js
const nextJest = require('next/jest');

const createJestConfig = nextJest({
  // next.config.jsとテスト環境用の.envファイルが配置されたディレクトリをセット
  dir: './',
});

// Jestのカスタム設定を設置する場所。従来のプロパティはここで定義
const customJestConfig = {
  // テストファイルの検索対象ディレクトリ
  moduleDirectories: ['node_modules', '<rootDir>/'],
  // テストファイルの検索パターン
  testMatch: ['**/*.test.ts', '**/*.test.tsx'],
  // テスト環境を指定
  testEnvironment: 'jest-environment-jsdom',
  // jest-dom等の追加セットアップを記述したファイルを指定
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  // モジュールの別名を定義（tsconfig.jsonのpathsと合わせる）
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1',
  },
  // カバレッジレポートの設定
  collectCoverage: true,
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.{js,jsx,ts,tsx}',
  ],
};

// createJestConfigを定義することによって、本ファイルで定義された設定がNext.jsの設定に反映される
module.exports = createJestConfig(customJestConfig);

// jest.setup.js
import '@testing-library/jest-dom';

// テスト時にWindow.matchMediaのモックが必要な場合
Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: jest.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: jest.fn(),
    removeListener: jest.fn(),
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  })),
});
```


```
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

```
// src/utils/test-utils.tsx
import React from 'react';
import { render, RenderOptions } from '@testing-library/react';

// 必要に応じてProviderを追加
const AllTheProviders = ({ children }: { children: React.ReactNode }) => {
  return (
    <>
      {/* Add providers here */}
      {children}
    </>
  );
};

const customRender = (
  ui: React.ReactElement,
  options?: Omit<RenderOptions, 'wrapper'>
) => render(ui, { wrapper: AllTheProviders, ...options });

// re-export everything
export * from '@testing-library/react';

// override render method
export { customRender as render };
```

```
// src/components/Button/Button.tsx
import React from 'react';

interface ButtonProps {
  onClick?: () => void;
  children: React.ReactNode;
  disabled?: boolean;
}

export const Button = ({ onClick, children, disabled = false }: ButtonProps) => {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 disabled:opacity-50"
    >
      {children}
    </button>
  );
};

// src/components/Button/Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders button with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>Click me</Button>);
    expect(screen.getByText('Click me')).toBeDisabled();
  });
});
```

```
# すべてのテストを実行
npm test

# ウォッチモードでテストを実行
npm run test:watch

# カバレッジレポートを生成
npm run test:coverage
```
