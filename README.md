# .NET-JSON-Transformer
A Visual Basic .NET implementation that converts a JSON literal into a .NET XDocument

To use the file simply add it to your project and call the JSON.Parse method.

## Syntax
`Public Function Parse(ByVal source As String) As XDocument`

**Parameters**
- *source*
  - Type: System.String
  - The JSON literal to be converted.

- Return Value
  - Type: System.Xml.Linq.XDocument
  - An XML representation of the JSON literal. If the JSON is not parsable, then the method returns Nothing.
  
## Remarks
  * The parser ignores whitespace, essentially minfying the JSON. For example, if the JSON literal is:
    ``` json
    {
        "glossary": {
            "title": "example glossary",
            "GlossDiv": {
                "title": "S",
                "GlossList": {
                    "GlossEntry": {
                        "ID": "SGML",
                        "SortAs": "SGML",
                        "GlossTerm": "Standard Generalized Markup Language",
                        "Acronym": "SGML",
                        "Abbrev": "ISO 8879:1986",
                        "GlossDef": {
                            "para": "A meta-markup language, used to create markup languages such as DocBook.",
                            "GlossSeeAlso": ["GML", "XML"]
                        },
                        "GlossSee": "markup"
                    }
                }
            }
        }
    }
    ```
  Then it gets parsed as:
  ``` json
  {"glossary":{"title":"example glossary","GlossDiv":{"title":"S","GlossList":{"GlossEntry":{"ID":"SGML","SortAs":"SGML","GlossTerm":"Standard Generalized Markup Language","Acronym":"SGML","Abbrev":"ISO 8879:1986","GlossDef":{"para":"A meta-markup language, used to create markup languages such as DocBook.","GlossSeeAlso":["GML","XML"]},"GlossSee":"markup"}}}}}
  ```
  
  * The returned XML is a 1-to-1 translation from the JSON. Using the same example as above, the resulting XML would be:
    ```
    <object>
      <item>
        <key>glossary</key>
        <value>
          <object>
            <item>
              <key>title</key>
              <value>
                <string>example glossary</string>
              </value>
            </item>
            <item>
              <key>GlossDiv</key>
              <value>
                <object>
                  <item>
                    <key>title</key>
                    <value>
                      <string>S</string>
                    </value>
                  </item>
                  <item>
                    <key>GlossList</key>
                    <value>
                      <object>
                        <item>
                          <key>GlossEntry</key>
                          <value>
                            <object>
                              <item>
                                <key>ID</key>
                                <value>
                                  <string>SGML</string>
                                </value>
                              </item>
                              <item>
                                <key>SortAs</key>
                                <value>
                                  <string>SGML</string>
                                </value>
                              </item>
                              <item>
                                <key>GlossTerm</key>
                                <value>
                                  <string>Standard Generalized Markup Language</string>
                                </value>
                              </item>
                              <item>
                                <key>Acronym</key>
                                <value>
                                  <string>SGML</string>
                                </value>
                              </item>
                              <item>
                                <key>Abbrev</key>
                                <value>
                                  <string>ISO 8879:1986</string>
                                </value>
                              </item>
                              <item>
                                <key>GlossDef</key>
                                <value>
                                  <object>
                                    <item>
                                      <key>para</key>
                                      <value>
                                        <string>A meta-markup language, used to create markup languages such as DocBook.</string>
                                      </value>
                                    </item>
                                    <item>
                                      <key>GlossSeeAlso</key>
                                      <value>
                                        <array>
                                          <string>GML</string>
                                          <string>XML</string>
                                        </array>
                                      </value>
                                    </item>
                                  </object>
                                </value>
                              </item>
                              <item>
                                <key>GlossSee</key>
                                <value>
                                  <string>markup</string>
                                </value>
                              </item>
                            </object>
                          </value>
                        </item>
                      </object>
                    </value>
                  </item>
                </object>
              </value>
            </item>
          </object>
        </value>
      </item>
    </object>
    ```

  * The parser does not parse to the exact specifications of the EBNF found on http://www.json.org/ the following list the deviations in this parser:
    * Number: Checks if the number starts with either a positive sign or negative sign
    * Boolean: Checks for "true" or "false" based on case-insensitivity
    * Null: Checks for "null" based on case-insensitivity
    * String: Does not check for "\u" followed by 4 hexadecimal characters as an escape character

## Examples and Demo:
  The following example demonstrates the Parse method.
  
  ``` vb.net
    Module Module1

        Sub Main()
            Dim input As String = <input>
                                      {
                                        "glossary": {
                                            "title": "example glossary",
		                                    "GlossDiv": {
                                                "title": "S",
			                                    "GlossList": {
                                                    "GlossEntry": {
                                                        "ID": "SGML",
					                                    "SortAs": "SGML",
					                                    "GlossTerm": "Standard Generalized Markup Language",
					                                    "Acronym": "SGML",
					                                    "Abbrev": "ISO 8879:1986",
					                                    "GlossDef": {
                                                            "para": "A meta-markup language, used to create markup languages such as DocBook.",
						                                    "GlossSeeAlso": ["GML", "XML"]
                                                        },
					                                    "GlossSee": "markup"
                                                    }
                                                }
                                            }
                                        }
                                    }
                                  </input>.Value.ToString()
            Dim output As XDocument = JSON.Parse(input)
            Console.WriteLine(output) : Console.ReadLine()
        End Sub

    End Module
  ```
Fiddle: https://dotnetfiddle.net/3ndfIJ
