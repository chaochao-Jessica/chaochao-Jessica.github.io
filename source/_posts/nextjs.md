# NEXT.JS入门实践

客户端和服务端代码 同构 ： 一套代码既可以跑在客户端也可以跑在服务端渲染一致
优点：
•	Next.js英文文档 Next.js中文文档 是一个轻量级的 React 服务端渲染应用框架
•	默认支持服务端渲染
•	自动根据页面进行代码分割
•	基于页面的客户端路由方案
•	基于 Webpack 的开发环境，支持热模块替换
•	可以跟Koa或者其它Node.js服务器进行集成
•	支持 Babel 和 Webpack 的配置项定制
•	静态文件服务 public

## 创建一个 Next.js 应用

通过执行 create-next-app创建应用，cd 进入您要在其中创建应用程序的目录，输入命令
npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
运行开发服务器
cd进入名为 nextjs-blog 的新目录
cd nextjs-blog
运行命令，这将在 3000 端口上启动 Next.js 应用程序的“开发服务器”。在浏览器中打开 http://localhost:3000 就会看到页面了。
npm run dev
编辑页面
确保 Next.js 开发服务器仍在运行
用你的文本编辑器打开 pages/index.js 文件，编辑内容后保存，浏览器会自动重新加载并更新页面
创建页面
Page 就是 React 组件， 放在 pages 目录下

## 路由
1、文件系统路由，每个 page使用其文件名在pages的目录，作为路由（route）。
index路由，路由器会自动将命名为index的文件路由到目录的根目录。
• pages/index.js → /
• pages/blog/index.js → /blog
嵌套路由，
• pages/blog/first-post.js → /blog/first-post
• pages/dashboard/settings/username.js → /dashboard/settings/username
http://localhost:3000/blog/first-post即可查看页面
动态路由的 pages（页面）。例如pages/posts/[id].js 的文件，那么就可以通过 posts/1、posts/2 等类似的路径进行访问。
要匹配动态段，可以使用括号语法。这允许您匹配命名参数
• pages/blog/[slug].js → /blog/:slug (/blog/hello-world)
• pages/[username]/settings.js → /:username/settings (/foo/settings)
• pages/post/[...all].js → /post/* (/post/2020/id/title)
## 用Link实现页面间导航
2、使用Next.js的组件Link 跳转页面，实现组件切换局部刷新。 代替了<a href=''>触发浏览器refresh整个页面。
import Link from 'next/link';
//使用<Link>里面放需要一个<a>包裹连接</a></Link>
export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
如果需要给路由传参数，则使用query的形式
Client-side Navigation
## 代码分割和预取
3、自动代码分割。相互独立，每个页面只加载该页面所需要的内容。
4、Prefetching： 如果使用了Link ，出现在浏览器中，Linked page 在后台就会预先获取。点击链接转换几乎是及时的。
资源、元数据和css
## 静态资源
根目录创建public文件夹，里面的静态资源可以像pages一样通过目录路径访问。
修改页面的metadata 
组件中引入Head组件, 在组件中添加
import Head from 'next/head';
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      …
    </>
  )
}
## CSS样式
支持“CSS-in-JS”库 styled-jsx，写在render中，格式为<style jsx>`...`</style>，注意中间是模板字符串。
组件内使用CSS Modules
首先根目录下创建components文件夹，里面创建layout.js文件，作为外层组件。
export default function Layout({ children }) {
  return <div>{children}</div>
}
在pages/posts/first-post.js, import Layout 
import Head from 'next/head'
import Link from 'next/link'
import Layout from '../../components/layout'
export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  )
}
支持添加css modules,文件必须以.modules.css结尾.
在components文件夹下创建layout.module.css内容为
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
在layout.js中引入
import styles from './layout.module.css'
export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
全局styles
在pages中添加App.js，这个App组件是顶级组件，在所有不同的页面中都是通用的。例如，当在页面之间导航时，你可以使用这个App组件来保持状态。
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
重启服务 npm run dev
创建顶级目录styles文件夹，新建global.css，然后在pages/_app.js引入，样式将在子组件中都生效。
Next.js使用postCSS解析css，可以添加配置文件postcss.config.js，自定义配置。

## 预渲染和数据获取
• 预呈现：默认情况下，Next.js预渲染每个页面。这意味着Next.js提前为每个页面生成HTML，而不是全部由客户端JavaScript完成。预呈现可以带来更好的性能和SEO。
• 水合：预呈现的HTML都与该页面所需的最小JavaScript代码相关联。当页面被浏览器加载时，它的其他JavaScript代码会运行，使页面可完成全部交互。
• 查看是否预呈现：禁用浏览器JavaScript，看到你的应用程序是在没有JavaScript的情况下呈现的。这是因为Next.js已经将应用程序预渲染成静态HTML，允许您在不运行JavaScript的情况下查看应用程序UI。而没有Next.js的react应用,是看不到应用的。
预呈现的两种形式:静态生成和服务器端呈现。
不同之处在于它为页面生成HTML的时间。
• 静态生成：是在构建build时生成HTML的预呈现方法。然后在每次请求重用预呈现的HTML。
• 服务器端呈现：是在每次页面请求时，重新生成页面的 HTML预呈现方法 。
注意：在开发模式下(当你运行npm run dev或yarn dev时)，每个页面都会在每个请求上预渲染——即使是那些使用静态生成的页面。
何时使用静态生成or服务端渲染：你应该问自己:“我可以在用户请求之前预先渲染这个页面吗?”如果答案是肯定的，那么您应该选择静态生成。如果您不能在用户请求之前预呈现页面，也许您的页面显示了频繁更新的数据，并且页面内容在每次请求时都发生变化。在这种情况下，可以使用服务器端呈现。它会慢一些，但预渲染的页面将始终是最新的。或者您可以跳过预呈现，使用客户端JavaScript来填充频繁更新的数据。
有数据和没有数据的静态生成。
到目前为止都是没有数据的静态生成，
有数据的页面需要等获取数据后，再生成页面。

### 获取数据
getStaticProps以及如何使用它将外部博客数据导入索引页面。在导出组件的page里，导出一个asyn function 叫做getStaticProps
• getStaticProps在生产环境的build时运行，并且……
• 在函数内部，可以获取外部数据并将其作为props发送到页面。
export default function Home(props) { ... }
export async function getStaticProps(context) { 
    // 获取数据
  const data = await fetch(...);
  // The value of the `props` key will be passed to the `Home` component
  return {
    props: ...
  }
}
getInitialProps是组件的静态方法，无论服务端渲染还是客户端渲染都会调用。
获取数据后，组件的props就能获取到getInitialProps return 的对象，render 的时候就能直接使用了。当服务端渲染时，getInitialProps将会把return数据序列化，就像JSON.stringify
getStaticProps会帮你告诉Next.js这个页面依赖一些数据，所以“当你在构建时预渲染这个页面时，一定要先解决它们!”
注意：在开发模式中，getStaticProps会在每个请求上运行。
现在，我们将使用文件系统向应用程序添加博客数据。每一篇博客文章都是一个markdown文件。
创建一个新的顶级目录posts(这与pages/posts不同)，在内部，创建两个文件:pre-rendering.md和ssg-ssr.md作为数据，然后用getStaticProps获取数据更新pages/index.js。
getStaticProps 细节
getStaticProps里除了可以使用文件系统获取数据，还可以使用API或者直接查询数据库。因为getStaticProps只在服务器端运行。它永远不会在客户端运行。它甚至不会包含在浏览器的JS包中。这意味着您可以编写诸如直接数据库查询之类的代码，而无需将它们发送到浏览器。
Development vs. Production
• 在开发中(npm run dev或yarn dev)， getStaticProps会在每个请求request上运行。
• 在生产中，getStaticProps在构建build时运行。
因为它是在构建时运行的，所以您不能使用仅在请求期间可用的数据，如查询参数或HTTP头。
从页面导出
getStaticProps只能从page导出。您不能从非页面文件中导出它。产生这种限制的原因之一是，React需要在呈现页面之前获得所有必需的数据。
### 有数据的服务端预渲染
需要请求时获取数据怎么办
如果不能在用户请求之前预呈现页面，那么静态生成就不是一个好主意。也许您的页面显示了频繁更新的数据，并且页面内容在每次请求时都发生变化。在这种情况下，您可以尝试服务器端呈现或跳过预呈现。
如果你需要在请求时而不是在构建时获取数据，你可以尝试服务器端渲染:
在page中 用export getServerSideProps代替getStaticProps
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    }
  }
}
因为getServerSideProps是在请求时调用的，所以它的参数(context)包含特定于请求的参数。
只有当您需要预呈现一个必须在请求时获取数据的页面时，您才应该使用getServerSideProps。到第一个字节的时间Time to first byte(TTFB)将比getStaticProps慢，因为服务器必须对每个请求计算结果。而且如果没有额外的配置，CDN无法缓存结果。
### 客户端渲染
如果你不需要预渲染数据，你也可以使用以下策略(称为客户端渲染):
静态生成(预渲染)不需要外部数据的页面部分。
加载页面时，使用JavaScript从客户机获取外部数据，并填充其余部分。
例如，这种方法适用于用户指示板页面。因为指示板是一个私有的、特定于用户的页面，所以SEO是无关的，并且页面不需要预先渲染。数据经常更新，这就需要在请求时获取数据。

## 动态路由
我们已经用博客数据填充了索引页面，但是还没有创建单独的博客页面(下面是预期的结果)。我们希望这些页面的URL依赖于博客数据，这意味着我们需要使用动态路由。
getStaticPaths静态生成带有动态路由的页面
我们希望每一个post的路径为/posts/<id>，其中<id>是markdown文件的名称。我们有两个文件 ssg-ssr.md 和 pre-rendering.md, 我们希望他们的路径是 /posts/ssg-ssr and /posts/pre-rendering.
• 我们将在pages/posts下创建一个名为[id].js的页面，编写代码来渲染post页面，像创建其他页面一样，
• 并从这个page中 export function getStaticPath 
• 最后，我们需要再次实现getStaticProps—这一次，用给定的id为博客文章获取必要的数据。getStaticProps将给出包含id的参数(因为文件名是[id].js)。
import Layout from '../../components/layout'
export default function Post() {
  return <Layout>...</Layout>
}
export async function getStaticPaths() {
  // Return a list of possible value for id
  return ['id', 'id']
}
export async function getStaticProps({ params }) {
  // 根据params.id，获取blog post数据 
}