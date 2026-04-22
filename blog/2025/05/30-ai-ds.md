# Ai Ds (2025-05-30)

## getting-started/gitbook-ds/README.md




# Level 1: Main Heading

## Level 2: Tables

### Level 3: Basic Table

| Syntax    | Description |
| --------- | ----------- |
| Header    | Title       |
| Paragraph | Text        |

### Level 3: Aligned Table (GitBook supports this)

| Left-Aligned | Center-Aligned | Right-Aligned |
| :----------- | :------------: | ------------: |
| Cell A       |     Cell B     |        Cell C |
| `code`       |    **bold**    |      _italic_ |

### Table from xml

<table>
    <tr>
        <th>A</th>
        <th>B</th>
        <th>C</th>
    </tr>
    <tr>
        <td>1</td>
        <td>2</td>
        <td>3</td>
    </tr>
    <tr>
        <td>4</td>
        <td>5</td>
        <td>6</td>
    </tr>
    <tr>
        <td>7</td>
        <td>8</td>
        <td>9</td>
    </tr>
</table>

---

## Level 2: Mermaid Diagrams

### Level 3: Flowchart

```mermaid
flowchart TD
    A[Start] --> B{Decision}
    B -->|Yes| C[Action 1]
    B -->|No| D[Action 2]
```

# Testing rich-qoutes....

It should be easy to annotate:

**Info** Info

**Note** Note

**Tag** Tag

**Comment** Comment

**Hint** Hint

**Success** Success

**Warning** Warning

**Caution** Caution

**Danger** Danger

**Quote** Quote

# Links

- [Creating content - GitBook](https://gitbook.com/docs/creating-content/formatting)

- Added [richqoutes](https://github.com/erixtekila/gitbook-plugin-richquotes)


## getting-started/mermaid/README.md



# Mermaid digrams

During a recent AI-course AI, the topic or re-factoring came up.
Specifically to convert images or drawing to mermaid-syntax.

So I took that opertunity to:

1. Give Gemini an image
2. Ask it to convert it to mermaid syntax

## Result

```mermaid
graph TD
    AAA_Root["AAA"]

    %% Authentication Branch
    Node_AuthN["Authentication"]
    AAA_Root --> Node_AuthN

    subgraph S_Authentication [Authentication]
        Node_AuthN_GroupsContainer["Groups (container)"]
        Node_AuthN_GroupAdmin["Group<br/>name=&quot;admin&quot;<br/>users=&quot;joe&quot;"]
        Node_AuthN_GroupLamers["Group<br/>name=&quot;lamers&quot;<br/>users=&quot;steve&quot;"]
        Node_AuthN_UsersContainer["Users (container)"]
        Node_AuthN_UserJoe["User<br/>name=&quot;joe&quot;<br/>password=&quot;xyz&quot;"]
        Node_AuthN_UserSteve["User<br/>name=&quot;steve&quot;<br/>password=&quot;zyx&quot;"]
    end
    Node_AuthN --> Node_AuthN_GroupsContainer
    Node_AuthN_GroupsContainer --> Node_AuthN_GroupAdmin
    Node_AuthN_GroupsContainer --> Node_AuthN_GroupLamers
    Node_AuthN --> Node_AuthN_UsersContainer
    Node_AuthN_UsersContainer --> Node_AuthN_UserJoe
    Node_AuthN_UsersContainer --> Node_AuthN_UserSteve

    %% Authorization Branch
    Node_AuthZ["Authorization"]
    AAA_Root --> Node_AuthZ

    subgraph S_Authorization [Authorization]
        Node_AuthZ_GroupsContainer["Groups (container)"]

        Node_AuthZ_GroupAdmin["Group<br/>name=&quot;admin&quot;"]
        Node_AuthZ_Admin_CLI["CLI"]
        Node_AuthZ_Admin_CLI_PathsContainer["paths (container)"]
        Node_AuthZ_Admin_CLI_PathExecute["path=/ <br/>access=execute"]

        Node_AuthZ_Admin_Netconf["netconf"]
        Node_AuthZ_Admin_Netconf_PathsContainer["paths (container)"]
        Node_AuthZ_Admin_Netconf_PathExecute["path=/ <br/>access=execute"]


        Node_AuthZ_GroupLamers["Group<br/>name=&quot;lamers&quot;"]
        Node_AuthZ_Lamers_Netconf["netconf"]
        Node_AuthZ_Lamers_Netconf_PathsContainer["paths (container)"]
        Node_AuthZ_Lamers_Netconf_PathDhcpWrite["path=/dhcp <br/>access=write"]
        Node_AuthZ_Lamers_Netconf_PathAaaRead["path=/aaa <br/>access=read"]
    end
    Node_AuthZ --> Node_AuthZ_GroupsContainer

    Node_AuthZ_GroupsContainer --> Node_AuthZ_GroupAdmin
    Node_AuthZ_GroupAdmin --> Node_AuthZ_Admin_CLI
    Node_AuthZ_Admin_CLI --> Node_AuthZ_Admin_CLI_PathsContainer
    Node_AuthZ_Admin_CLI_PathsContainer --> Node_AuthZ_Admin_CLI_PathExecute

    Node_AuthZ_GroupAdmin --> Node_AuthZ_Admin_Netconf
    Node_AuthZ_Admin_Netconf --> Node_AuthZ_Admin_Netconf_PathsContainer
    Node_AuthZ_Admin_Netconf_PathsContainer --> Node_AuthZ_Admin_Netconf_PathExecute

    Node_AuthZ_GroupsContainer --> Node_AuthZ_GroupLamers
    Node_AuthZ_GroupLamers --> Node_AuthZ_Lamers_Netconf
    Node_AuthZ_Lamers_Netconf --> Node_AuthZ_Lamers_Netconf_PathsContainer
    Node_AuthZ_Lamers_Netconf_PathsContainer --> Node_AuthZ_Lamers_Netconf_PathDhcpWrite
    Node_AuthZ_Lamers_Netconf_PathsContainer --> Node_AuthZ_Lamers_Netconf_PathAaaRead
```

## Experience

Great! That saves us a lot of time - and we do not have to ask a junior to do
this for us.

The result is pretty good, and other advantages - such as diffing the code
become easier this way.

## GitBook

After issue [Issue 3271](https://github.com/GitbookIO/gitbook/issues/3271) was
fixed - now we can switch between dark- and normal-mode and the rendering looks
good.

## Admonitions

You need to use hints... not very pretty...


{% hint style="info" %}
**Info hints** are great for showing general information, or providing tips and tricks.
{% endhint %}

{% hint style="success" %}
**Success hints** are good for showing positive actions or achievements.
{% endhint %}

{% hint style="warning" %}
**Warning hints** are good for showing important information or non-critical warnings.
{% endhint %}

{% hint style="danger" %}
**Danger hints** are good for highlighting destructive actions or raising attention to critical information.
{% endhint %}




## Links

- [Mermaid Gitbook Examples](https://raw.githubusercontent.com/mermaidjs/mermaid-gitbook)

