private void GenerateCode(ReportData data)
{
	var code = new StringBuilder();
	foreach (var prop in data.GetType().GetProperties())
	{
		switch (prop.GetValue(data))
		{
			case null:
				code.AppendLine($"Assert.Null(data.{prop.Name});");
				break;
			case Array array when array.Length > 0:
				code.AppendLine($"Assert.Equal({array.Length}, data.{prop.Name}.Length);");
				for (var i = 0; i < array.Length; i++)
				{
					switch (array.GetValue(i))
					{
						case null:
							code.AppendLine($"Assert.Null(data.{prop.Name}[{i}]);");
							break;
						case string str:
							code.AppendLine($"Assert.Equal(\"{str}\", data.{prop.Name}[{i}]);");
							break;
						case Array innerArr when innerArr.Length > 1:
							for (var j = 0; j < innerArr.Length; j++)
								code.AppendLine(
									$"Assert.Equal(\"{innerArr.GetValue(j)}\", data.{prop.Name}[{i}][{j}]);");
							break;
						case Array innerArr:
							code.AppendLine($"Assert.Equal(0, data.{prop.Name}[{i}].Length);");
							break;
						case object val:
							foreach (var arrProp in val.GetType().GetProperties())
								code.AppendLine(
									$"Assert.Equal(\"{arrProp.GetValue(val)}\", data.{prop.Name}[{i}].{arrProp.Name});");
							break;
					}
				}

				break;
			case Array _:
				code.AppendLine($"Assert.Equal(0, data.{prop.Name}.Length);");
				break;
			case Enum enm:
				code.AppendLine($"Assert.Equal({enm.GetType().Name}.{enm}, data.{prop.Name});");
				break;
			case string str:
				code.AppendLine($"Assert.Equal(\"{str}\", data.{prop.Name});");
				break;
			default:
				code.AppendLine($"{prop.Name} ???");
				break;
		}
	}

	WriteLine(code);
}
