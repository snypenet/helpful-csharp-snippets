using System.Text.Json;
using System.Text.Json.Nodes;

namespace Utils
{
    public static class JsonFlattener
    {
        public static Dictionary<string, string?> Flatten(string jsonData)
        {
            var node = JsonNode.Parse(jsonData);
            var map = new Dictionary<string, string?>();
            WalkTheTree(map, "", node);
            return map;
        }

        private static void WalkTheTree(Dictionary<string, string?> map, string currentKey, JsonNode? node)
        {
            if (node == null)
            {
                map[currentKey] = null;
            }
            else
            {
                var kind = node.GetValueKind();
                switch (kind)
                {
                    case JsonValueKind.Object:
                        var jsonObject = node.AsObject();
                        foreach (var property in jsonObject)
                        {
                            if (currentKey == "")
                            {
                                WalkTheTree(map, property.Key, property.Value);
                            }
                            else
                            {
                                WalkTheTree(map, $"{currentKey}.{property.Key}", property.Value);
                            }
                        }
                        break;
                    case JsonValueKind.Array:
                        var jsonArray = node.AsArray();
                        for (int i = 0; i < jsonArray.Count; i++)
                        {
                            WalkTheTree(map, $"{currentKey}[{i}]", jsonArray[i]);
                        }
                        break;
                    case JsonValueKind.String:
                    case JsonValueKind.Number:
                        string stringValue = node.AsValue().ToString();
                        map[currentKey] = stringValue;
                        break;
                    case JsonValueKind.Undefined:
                    case JsonValueKind.Null:
                        map[currentKey] = null;
                        break;
                    case JsonValueKind.True:
                        map[currentKey] = "true";
                        break;
                    case JsonValueKind.False:
                        map[currentKey] = "false";
                        break;

                }
            }
        }
    }
}
