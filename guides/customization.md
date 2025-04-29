# Customization Guide

This guide explains how to customize MCP templates for your organization's specific needs.

## Why Customize?

- **Industry-specific requirements** - Add fields relevant to your domain
- **Organizational standards** - Align with your company's best practices
- **Workflow integration** - Adapt to your team's existing processes
- **Schema specifics** - Focus on your particular database architecture

## Customization Approaches

### Modifying Existing Templates

1. **Start with a base template** from the `/templates` directory
2. **Add, remove, or modify sections** to suit your needs
3. **Test with your team** to ensure it provides the right context
4. **Document changes** for future reference

### Creating New Templates

1. **Identify the specific use case** the template will address
2. **Outline the required context** for that use case
3. **Structure the template** with clear sections and instructions
4. **Include examples** to show how to fill it out correctly

## Template Structure Best Practices

- **Keep it concise** - AI assistants work best with relevant, focused information
- **Use clear section headers** - Makes it easy to navigate and fill out
- **Include optional sections** - Mark them clearly for users to decide
- **Add inline guidance** - Help users understand what to include in each section

## Example: Customizing for Data Warehouse Team

```markdown
# Data Warehouse Optimization MCP Template

## Query Context
[Describe the ETL process this query is part of]

## Table Information
[Include warehouse-specific details like partitioning strategy]

## Performance Requirements
[Add warehouse-specific metrics like load window constraints]

## Current Issues
[Describe batch processing concerns or dependencies]
```

## Organization-Specific Additions

Consider adding these sections for enterprise environments:

- **Security Context** - Data classification, access requirements
- **Compliance Requirements** - Regulatory considerations
- **Dependencies** - Upstream/downstream processes
- **Business Impact** - Criticality of the query to operations

## Versioning Templates

As your templates evolve:

1. **Use semantic versioning** (e.g., v1.0.0)
2. **Document changes** between versions
3. **Maintain backwards compatibility** where possible
4. **Provide migration guides** for major changes

## Next Steps

- Share your custom templates in your organization's template repository
- Gather feedback on template effectiveness
- Refine based on actual usage patterns
- Check [governance model](governance.md) for long-term maintenance strategy 