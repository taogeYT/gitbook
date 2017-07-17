    通用输入帐号密码xpath表达式，如下示例通过awesome登录页演示
    $x('//input[not(@type="hidden")]')
    $x('(//input[not(@type="hidden")])[1]')
    $x('(//input[not(@type="hidden")])[2]')
    $x('(//input[not(@type="hidden")])[2]/following::*[@type="submit"]')


    //td[contains(text(),'行业')]  模糊查询
    /following::*[1] 选取当前元素的下一个同胞元素

    .//text() 选取已选节点下的所有text内容

    xpath返回都是列表，没有匹配到就返回空列表[]

    lxml使用

    ele.get('id')获取属性
    ele.tag
    ele.text
